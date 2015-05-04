--------------------------------------------------
Overall Options
--------------------------------------------------

-v
    Print (on standard error output) the commands executed to run the stages of compilation.
    Also print the version number of the compiler driver program and of the preprocessor and the compiler proper.

-###
    Like -v except the commands are not executed and arguments are quoted unless they contain only alphanumeric characters or "./-_".
    This is useful for shell scripts to capture the driver-generated command lines.

--help
    Print (on the standard output) a description of the command-line options understood by gcc.
    If the -v option is also specified then --help is also passed on to the various processes invoked by gcc,
    so that they can display the command-line options they accept.
    If the -Wextra option has also been specified (prior to the --help option),
    then command-line options that have no documentation associated with them are also displayed.

--help={class|[^]qualifier}[,...]
    Print (on the standard output) a description of the command-line options understood by the compiler that fit into all specified classes and qualifiers.

    These are the supported classes:

    optimizers
        Display all of the optimization options supported by the compiler.

    warnings
        Display all of the options controlling warning messages porduced by the compiler.

    target
        Display target-specific options.
        Unlike the --target-help option however, target-specific options of the linker and assembler are not displayed.
        This is because those tools do not currently support the extended --help= syntax.

    params
        Display the values recognized by the --param option.

    language
        Display the options supported for language, where language is the name of one of the languages supported in this version of GCC.

    common
        Display the options that are common to all languages.

    These are the supported qualifiers:

    undocumented
        Display only those options that are undocumented.

    joined
        Display options taking an argument that appears after an equal sign in the same continuous piece of text, such as: --help=target.

    separate
        Display options taking an argument that appears as a separate word following the original option, such as: -o output-file.

    If the -Q option appears on the command line before the --help= option, then the descriptive text displayed by --help= is changed.
    Instead of describing the displayed options, and indication is given as to whether the option is enabled, disabled or
    set to a specific value (assuming that the compiler knows this at the point where the --help= option is used).

--help=target
-Q --help=target

--target-help
    Print (on the standard output) a description of target-specific command-line options for each tool.
    For some targets extra target-specific information may also be printed.


--------------------------------------------------
Debugging Options
--------------------------------------------------
-fdump-tree-all
-fopt-info
-fopt-info-options[=file]
