# CBT586
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. 
Due to amazing work by Alison Zhang and Jake Choi repos are no longer deleted.

```
//***FILE 586 is from Robin Murray, and contains several REXX execs *   FILE 586
//*           and a panel, which allow invocation of any ISPF       *   FILE 586
//*           application that you can invoke with a START command, *   FILE 586
//*           The ISPF command which this service creates, is       *   FILE 586
//*           called SS.                                            *   FILE 586
//*                                                                 *   FILE 586
//*     A fairly detailed description of the use of this service    *   FILE 586
//*     follows here:                                               *   FILE 586
//*                                                                 *   FILE 586
//*     To use this service, simply enter 'SS appname' where        *   FILE 586
//*     appname is anything you can enter on the ISPF 'START'       *   FILE 586
//*     command service:                                            *   FILE 586
//*                                                                 *   FILE 586
//*       > an option off the main menu (1.3.4)                     *   FILE 586
//*       > a TSO command (TSO %TLMS)                               *   FILE 586
//*       > an ISPF command from a command table                    *   FILE 586
//*       > an ISPF dialog service (CMD(%SDSF))                     *   FILE 586
//*       > etc. etc.                                               *   FILE 586
//*                                                                 *   FILE 586
//*     The SS command will start the new service in a new          *   FILE 586
//*     session, and create a default screen name to match it       *   FILE 586
//*     (using the SCRNAME ISPF command). The screen name it        *   FILE 586
//*     creates depends on what command you pass to it. It must     *   FILE 586
//*     parse out of the parms a valid screen name that it can      *   FILE 586
//*     pass to the SCRNAME command.                                *   FILE 586
//*                                                                 *   FILE 586
//*       > for an option such as 1.3.4, the screen name will       *   FILE 586
//*         be Q134                                                 *   FILE 586
//*       > for a tso command such as TSO %TLMS, it will be         *   FILE 586
//*         TLMS                                                    *   FILE 586
//*       > for an command table command, it will be the            *   FILE 586
//*         command name                                            *   FILE 586
//*       > for a dialog service such as CMD(%SDSF), it will be     *   FILE 586
//*         SDSF                                                    *   FILE 586
//*       > etc. etc.                                               *   FILE 586
//*                                                                 *   FILE 586
//*     There is no attempt to make the screen name unique. If      *   FILE 586
//*     there are duplicate names, you can override the name        *   FILE 586
//*     manually on the newly created screen.                       *   FILE 586
//*                                                                 *   FILE 586
//*     You can switch to the session you created by                *   FILE 586
//*     issuing the same command again, or issuing the SS           *   FILE 586
//*     command with the screen name:                               *   FILE 586
//*                                                                 *   FILE 586
//*       > SS 1.3.4 will take you back to the 1.3.4 screen         *   FILE 586
//*       > SS Q134 will also take you there                        *   FILE 586
//*       > SS TSO %TLMS will take you back to the tlms screen      *   FILE 586
//*       > SS TLMS will also take you there                        *   FILE 586
//*       > etc. etc.                                               *   FILE 586
//*                                                                 *   FILE 586
//*     Obviously, it's much easier to use the short screen         *   FILE 586
//*     name. The command will substitute a "SWAP xxx"              *   FILE 586
//*     command if it finds an active session with the same         *   FILE 586
//*     name.                                                       *   FILE 586
//*                                                                 *   FILE 586
//*     Entering 'SS' with no parameters is the same as             *   FILE 586
//*     entering the ISPF 'SWAP LIST' command. You can then         *   FILE 586
//*     see the screen names that this command created (as          *   FILE 586
//*     well as any others you created).                            *   FILE 586
//*                                                                 *   FILE 586
//*     So this one 'SS' command will combine the                   *   FILE 586
//*     functionality of the ISPF START, SCRNAME, SWAP              *   FILE 586
//*     LIST, and SWAP XXX commands, all in one easy to             *   FILE 586
//*     remember command.                                           *   FILE 586
//*                                                                 *   FILE 586
//*     But that's not all. This command will also support          *   FILE 586
//*     it's own variation of the ISPF commands table.              *   FILE 586
//*     Entering 'SS /' will bring up a table on which you          *   FILE 586
//*     can enter a screen name followed by any valid START         *   FILE 586
//*     command parameters. The 'SS' command will search            *   FILE 586
//*     this table for a match and if found, start the              *   FILE 586
//*     service using the specified screen name. Therefore,         *   FILE 586
//*     you could create entries such as:                           *   FILE 586
//*                                                                 *   FILE 586
//*       > TLMS     TSO %TLMS                                      *   FILE 586
//*       > SDSF     CMD(%SDSF)                                     *   FILE 586
//*       > DSL      1.3.4                                          *   FILE 586
//*       > PARM1    CMD(%EVBCMD 'SYS1.PARMLIB' E) NEWAPPL(ISR)     *   FILE 586
//*                                                                 *   FILE 586
//*     Then, entering 'SS TLMS' will start the TLMS                *   FILE 586
//*     command, or entering 'SS PARM1' will pop you into           *   FILE 586
//*     edit of sys1.parmlib with the correct applid, so            *   FILE 586
//*     that pfkeys will be properly set.  Entering 'SS             *   FILE 586
//*     DSL' will bring you to option 1.3.4 of the main             *   FILE 586
//*     menu (i'm assuming that this will be the dataset            *   FILE 586
//*     list panel).                                                *   FILE 586
//*                                                                 *   FILE 586
//*     At install time, the sysprog can create a default           *   FILE 586
//*     set of commands. the table is always saved in the           *   FILE 586
//*     ISPF profile dataset.  He can then move this table          *   FILE 586
//*     to a common ISPTLIB dataset so that everyone will           *   FILE 586
//*     have access to the default. After that, if anyone           *   FILE 586
//*     enters 'SS /', the default table will be copied to          *   FILE 586
//*     that user's ISPF profile dataset, and end up having         *   FILE 586
//*     their own personal copy of the table which they can         *   FILE 586
//*     customize to their heart's content.                         *   FILE 586
//*                                                                 *   FILE 586
//*     tip: The EVBCMD exec is shipped with this to use as         *   FILE 586
//*     a way to start the Edit/View/Browse ISPF service            *   FILE 586
//*     with a NEWAPPL(ISR) so that pfkeys will be set              *   FILE 586
//*     properly. To use this in the cmd table, put                 *   FILE 586
//*     "CMD(%EVBCMD 'my.dataset.name' x) NEWAPPL(ISR)"             *   FILE 586
//*     where x is either E V or B for Edit View or Browse.         *   FILE 586
//*                                                                 *   FILE 586
//*     tip: It's better to put a dialog service in the             *   FILE 586
//*     table such as CMD(), PGM() or PANEL() rather than           *   FILE 586
//*     an option or command off the main menu such as              *   FILE 586
//*     1.3.4, TSO TLMS etc. Since in the first case when           *   FILE 586
//*     you exit from the application you end the entire            *   FILE 586
//*     session, but in the second case you'll end up on            *   FILE 586
//*     the primary menu, so you'll have to hit end again           *   FILE 586
//*     to exit the session.                                        *   FILE 586
//*                                                                 *   FILE 586
//*     tip: By default, this exec will rename any temp             *   FILE 586
//*     screen names it finds whenever it's run. This will          *   FILE 586
//*     rename the initial session and any session started          *   FILE 586
//*     with the SPLIT or manual START command.  This               *   FILE 586
//*     behaviour can be turned off by setting InitPrefix           *   FILE 586
//*     to null in the StartApp routine above.                      *   FILE 586
//*                                                                 *   FILE 586
//*     tip: Go into ISPF general options and turn off              *   FILE 586
//*     "Always show split line" in order to put each               *   FILE 586
//*     session in a full screen.                                   *   FILE 586
//*                                                                 *   FILE 586
//*     tip: Replace any pfkey settings to use 'SWAP NEXT'          *   FILE 586
//*     instead of 'SWAP' so you can roll thru your                 *   FILE 586
//*     sessions.                                                   *   FILE 586
//*                                                                 *   FILE 586
//*     There's lots of room for performance improvments,           *   FILE 586
//*     one being to open the table when ISPF is started.           *   FILE 586
//*     That way the table won't have to be reopened every          *   FILE 586
//*     time, the use count will just be bounced. If the            *   FILE 586
//*     table isn't desired, you can comment out the call           *   FILE 586
//*     to TableLookaside at around line 60 above.                  *   FILE 586
//*                                                                 *   FILE 586
//*     For questions/comments contact Robin Murray at:             *   FILE 586
//*                                                                 *   FILE 586
//*     robin_murray@maritimelife.ca                                *   FILE 586
//*                 (semi-permanent contract pos) or                *   FILE 586
//*                                                                 *   FILE 586
//*     robinmurray@cyberdude.com (permanent email address)         *   FILE 586
//*                                                                 *   FILE 586
//*     tel: 902-453-7300 x4177                                     *   FILE 586
//*                                                                 *   FILE 586
```
