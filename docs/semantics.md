# CLIO Language semantics

In this section, we introduce the streaming semantics that allow CAPIO to transform a batch, file-based workflow into an in-situ workflow (i.e., a workflow where all steps are executed concurrently). To establish the file synchronization semantics between consecutive workflow steps, two temporal aspects should be considered:

- Determining when there are no further updates to the file, referred to as the `commit rule`
- Identifying when a consumer can safely start reading a portion of data written in the file, known as the `firing rule`

In the following, we describe the semantics of the `commit rule` and then the semantics of the `firing rule`  

## Commit rule

In the context of the producer-consumer paradigm, a file can be conceptualized as a data stream. The commit rule defines when a given data stream terminates, i.e., all consumers have received the so-called `end-of-stream` message, which tells them there will not be more data in input for that specific stream. The commit rule defines two distinct file commit behaviors:  

- `on-termination`: This behavior is applied in the traditional file-based execution of workflow steps. Upon termination of a workflow module, all produced data files are committed to the file system, becoming ready for consumption by all subsequent consumer modules.  
- `on-close`: This behavior allows a consumer module to initiate reading a file as soon as I/O operations from all producer modules working on that specific file are completed. The completion of these I/O operations is signaled by the `close()` system calls executed by all producer modules.  

Beyond these two primary file commit behaviors a third commit rule can be defined. We can consider a file committed when another file has been committed. This proves beneficial when the number of `open()` and `close()` system calls operations for a given file is not statically known. Instead, we are aware that the I/O operations can be considered concluded on a given file if another file has been committed. This additional commit behavior introduces a dependency among files in the commit rule, expanding opportunities to leverage temporal parallelism for I/O operations across different workflow steps.

Having introduced the commit rules, we shall define more formally the commit semantics for the producer(s) of a given `fileX`:

- `Commit on-Termination` (CoT): `fileX` is considered complete only when all producer modules associated with it have terminated.
- `Commit on-Close` (CoC): `fileX` is considered complete when all producer modules have definitively closed the file. With definitively closed, we intend that the file will not be - `Commit on-File` (CoF): The completion semantics of `fileX` are contingent upon the semantics of other files, e.g., `fileY`.

## Firing rule

The firing rule defines when consumer modules of a file are permitted to consume stream data items (i.e., the file's records) produced by producer modules. The file's data elements consumption can occur immediately or be delayed based on specific events. Practically speaking, the commit rule dictates when a particular data stream concludes, indicating that all consumers have received the so-called `end-of-stream` message, i.e., the notification that there will be no further data records for that specific stream of data.

The CAPIO coordination language defines two distinct firing rules:  

- `Fire on-commit` (FoC): whenever the commit rule is satisfied for a given file, the file is unequivocally ready to be consumed. In other words, the commit rule implies the firing rule for the entire file. This rule is identified in the CLIO coordination language file with the keyword `mode` and value `update`.
- `Fire no update` (FnU): file records already written by producer modules, can be consumed immediately: the file content is ready to be read as soon as data is written into the file. It is identified in the CLIO coordination language file with the keyword `mode` and value `no_update`.

There are two noteworthy scenarios. The first one arises when a consumer module attempts to open a file, which was specified in the CLIO configuration file, that has not yet been created. In this case, the CLIO runtime will halt the process executing the `open()` system call, and the `open()` will not return until the file is created. The second scenario is when a consumer module tries to read a portion of a file that has not been written yet. This behavior is nuanced as the `read()` system call may return fewer data elements (or even 0) than what was originally requested. In this case, the consumer process initiating the `read()` will be paused by CLIO runtime until one of the following conditions is met:

- the requested data is fully produced, and the `read()` system call returns the total number of bytes requested;
- all producer modules close the file (in the case of commit semantics being CoC) or terminate. Consequently, the `read()` system call will return the current number of bytes read, if any, or 0 to indicate the end of the file (EOF).
