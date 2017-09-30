#
# Installing the complete NonLinLoc software package:
#
# Written by Nima Nooshiri
# Last modification: 23-08-2017
#
# ----------------------------------------------------------------------


1- Unpack the source files NLL[ver]_src.tgz


2- Station labels in default NLL phase file output is limited to 6 characters.
   Change this in `src/GridLib.c` line 2905 (version 6.00) or line 3774
   (version 7.00):

    """
        // write observation part of phase line

        istat = fprintf(fpio,
                "%-6.6s %-4.4s %-4.4s .....
    """

    changing "%-6.6s" to larger values, e.g. "%-8.8s".


3- NLLoc writes some output files named `last.*` (e.g. last.in, last.stat etc)
   which can be problematic when it is run on multiple cores, since the name
   of the file (i.e. last) is common to all events. To stop NLLoc to write
   `last.*` files, comment following lines (put them between `/*` and `*/`):


    In file `src/NLLoc1.c`, lines:

        104-105 (v6.00) or 104-105 (v7.00)   FROM `char sys_command...` TO `char *chr`

        262-273 (v6.00) or 270-282 (v7.00)   FROM `if (!iSaveNone ...` TO `...sys_command);}`

        585-586 (v6.00) or 611-612 (v7.00)   FROM `sprintf ... last.stat ...` TO `system(sys_command);`

        602-603 (v6.00) or 628-629 (v7.00)   FROM `sprintf ... last.stat_totcorr ...` TO `system(sys_command);`  

        624-625 (v6.00) or 650-651 (v7.00)   FROM `sprintf ... last.stations ...` TO `system(sys_command);`


    In file `src/NLLocLib.c`, lines:

        848-858 (v6.00) or 924-934 (v7.00)   FROM `sprintf ... last.hyp ...` TO `system(sys_command);}`


4- If you use NonLinLoc version 6.00, change the following lines in
   `src/geo.h` file as bellow:

        #define FLATTENING 1.0/298.26 --> #define FLATTENING (1.0/298.26)

        #define KM2DEG (90.0/10000.0) --> #define KM2DEG (180.0/(PI*AVG_ERAD))

        #define DEG2KM (10000.0/90.0) --> #define DEG2KM (PI*AVG_ERAD/180.0)

   Following is the Anthony Lomax comment on why these changes should
   be applied:

        "// 20151106 AJL - changed km/deg scaling to be based on sphere
        with radius 6371, average Earth radius."


5- Compile NonLinLoc:

        make -R all
        (with gcc and Sun Solaris 2.6: make distrib)
        (with gcc and Linux (SuSe): gmake -R distrib)


6- Open .bashrc file and add NonLinLoc source path to PATH environment:

        export PATH=$PATH:/path/to/NLL7.00/src


7- Make a directory named "java" in your home directory and copy the
   following files there:

        SeismicityViewer50.jar
        Taup-2.1.2.jar   (or the latest version)
        TauP_NLL.jar


8- Open .bashrc file and set your "CLASSPATH" to *.jar files, all in
   "ONE SINGLE LINE":

        export CLASSPATH="
            /home/nooshiri/java/SeismicityViewer50.jar:
            /home/nooshiri/java/TauP-2.1.2.jar:
            /home/nooshiri/java/TauP_NLL.jar"

   NOTE: write the full path in the variable CLASSPATH, e.g. "/home/nooshiri/"
   instead of "~/" .


9- Source your .bashrc file:

        $ source ~/.bashrc

# ----------------------------------------------------------------------
# Done.
