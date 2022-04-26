# RuntimeParameters.jl

A module for managing runtime parameters for a CLI application in Julia. 
The application usage pattern is like git, with options and optional 
subcommands.

The philosophy of this module is that the parameter itself is the 
first-class entity. The developer defines the runtime parameters
by describing their attributes in a schema-checkable format (eg json or 
toml). The end-user can then specify their settings for each 
parameter via a combination of command-line options, environment 
variables and/or schema-checkable configuration files. 

The precedence for settings is:

1. Command-line options and arguments
2. Runtime environment variables
3. Configuration files in a developer-defined ordered list of locations
4. Application defaults

All of these are optional, with the constraint that every setting must 
have a defined default, either in the code (as a default value) or in
a required-at-runtime configuration file.

The API this module exports consists of these types and functions:

- Types:
  - `RuntimeParameter` - (`struct`) definition of a runtime parameter
  - `RuntimeParameterGroup` - (`NamedTuple` of `parameters` (`Dict`) and 
     `parameter_groups` (Dict))
  - `Subcommand` - (struct) a driver function, a `RuntimeParameterGroup` 
     and a vector of positional args corresponding to parameters in that 
     group
  - `SubcommandGroup` (`NamedTuple` with a `RuntimeParameterGroup`  and 
    `Dict` of `Subcommands`
  - `Setting` - (`struct`) the value of a `RuntimeParameter`, and 
    provenance trace of that value

- Functions:
  - get_settings(cmdline_args, runtimeparams, subcommands) returns a nametuple of settings
  - call_subcommand(settings) - calls the subcommand driver function and returns the result


