SDPCHAIN
===========

Seismology Data Preparation Module

Routines and classes providing a consistent interface for Python programs using
the SDPCHAIN protocol, which includes:

- Create or append to a process-steps.json file at each step
    - the process-steps file is read from the input directory and written
      to the output directory.  If both directories are the same the new step
      is appended to the existing file
- command-line arguments:
    - -d (base_dir)
    - -i (in_dir), can be relative to base directory
    - -o (out_dir), can be relative to base directory
    - optional (input_file) or (input_files), will have input directory pre-pended
    - optional -of (output_file) or -ofs (output_files), will have output dir pre-pended
    - optional -f, forces writing of output file if it already exists

- For now, these parameters must have exactly the above names in the files that
  use SDPCHAIN, so that setup_paths and ProcessStep process them correctly.
  The new ArgParser subclass should eliminate this need, but pre-populating
  the argparser with base_dir, in_dir and out_dir, as well as optionally 
  input_file or input_files, output_file or output_files, and/or -f
  
Classes
---------------------

`ProcessSteps` object to hold information for a process-steps file

Command-line Routines
---------------------

These routines perform common functions while following the
SDPCHAIN rules (process-steps file, -i, -o, -d)

`sdpstep`: run a standard command-line program 

`sdpcat`: concatenate binary files

process-steps.json
------------------

Contains information about each processing step, represented as an item in a
top-level list.  Each processing step item has ``application`` and ``execution``
fields specifiying, respectively, the application used and how it was executed.

Below is an example of a processing step file with only the required fields
and one step.

```json
{
  "steps": [
    {
      "application": {
        "description": "Concatenate files",
        "name": "sdpcat",
        "version": "0.73.post2"
      },
      "execution": {
        "date": "2021-03-31T12:45:13",
        "command_line": "sdpcat --ifs test.header.lch test.dat --of test.out",
        "parameters": {
          "directory_paths": {"base": ".", "input": ".", "output": "."},
          "input_files": ["test.header.lch", "test.dat"],
          "output_file": "test.out"
        },
        "messages": [],
        "exit_status": 0,
      }
    }
  ]
}
```




ToDo
----
Add `sdpcode`: change net and station codes?