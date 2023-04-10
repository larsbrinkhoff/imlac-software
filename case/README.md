Files from Rob Gingell.

To build these files, see [building.md](building.md).


The following are excerpts from private communiation:

---

[...] it turns out I do have the monitor software that we ran on
them. It also reminded me that after our PDP-10 was decommissioned we
did retain one of the IMLACs for a while as we used it to remotely
access another PDP-10 that hosted some of the research work that the
IMLACs supported for a time -- I'd completely forgotten that and
thought the IMLACs just vanished along with the -10.

The graphics software that ran on them, and, I believe, at one point
ran on the LDS-1 as well, was to support a graphics protocol for a
range of devices with varying device capability. The idea was to allow
there to be applications that could work on any of the devices we had,
and where the devices would be supplemented with software to perform
functions that the devices themselves couldn't do. In this model, the
LDS-1 was essentially capable of implementing the protocols almost
directly. The IMLACs could also do quite a bit but with some
augmentations. Our storage tube VT02 displays needed yet more
augmentation and of course could never do anything animated, and at
the bottom of the heirarchy something like a plotter was the least
capable device.

The primary author of that, Don Huff, wrote an early RFC discussing
some of the concepts. It's RFC285. It doesn't have many specifics and
is more of a basis for discussion, though the work he ultimately did
reflects some of the ideas that were in the RFC and the protocols were
implemented in the IMLAC monitor software I have. The whole system was
named the "Display Subroutine Library" (DSL) and the IMLAC-side
implementation was largely done by Ted Brenneman.

I think there was a utility that was used to bootstrap the IMLAC over
a serial line which was how we got the monitor into the device in
order to run it. Once it was installed, it behaved like a normal,
paginated terminal until it started receiving display protocol
sequences which would cause it to start generating graphics in
response.

---

Looking at them a little further there are a mix of source and binary
files. From some clues in the dates I think I may have tried operating
an IMLAC at Case as late as 1981. Our PDP-10 was decommissioned in
1975, and the software was saved from the TENEX file system. It was
transfered to a PDP-10 running TOPS-10 for some additional work. And
apparently saved off of that and loaded onto a DEC-20 at CWRU in
1981. I probably saved it onto a tape around 1985 and read those tapes
at Sun for the last time in the early 1990s. Through all those
transfers and the different programs employed is likely where some
artifacts came from.

---

They are "as is" from being extracted from the TOPS-20 DUMPER tape
they were on. The dates on the files appear to be correct for their
last modification, except for two files (pdsmon.old.1 and reloc.rel.1)
which claim to have been opened for write in November 2004. Not sure
what that was, though that's I think the day I left Sun so maybe I did
something in pulling them off of my workstation as I left.

For the source files we discussed the artifact of null padding. I've
noticed at least one file (chdef.mac.1) embeds the TENEX end-of-line
character (037, "^_") which was treated comparably to how *nix treat
'\n'. When DEC created TOPS-20 they eliminated that in favor of CRLF
pairs but I think lots of software would still handle it.

---

[...] we kept an IMLAC running through the early 1980s and the dates
on the file reflect the last things I was doing to get it hooked up to
a DEC-20 at CWRU. For example, the file 'pump.b36.2' is a BLISS-36
program that was interacting with the serial line we must have hooked
it to, which it named TTY153:. I'm guessing I was trying to get a
cable hooked up with the right flow control signals when I put that
hack together.

Some of the files appear untouched from our TENEX system and the
graphics environment I described. They were put onto a TOPS-10 system
(1976-1977) and then I had to make some modifications to get that to
work. And later we brought it back to TOPS-20 (1981) and I think it
ran for a while until something in the hardware broke irreparably.

The file 'pdsdef.mac.1' creates a bunch of symbols for the MACRO
assembler on the 10 to use. You could save pre-digested symbol tables
to speed assembly, these were "universal" files and 'pdsdef.unv.2' is
such a file. (It was a pretty normal convention for large pieces of
software on the PDP-10 to be started right after linking so they could
get initialized and then get saved in the "post-initialized but
pre-use" state to lower overhead. When you're using a timesharing
system of that vintage at 3 in the afternoon along with 50 other users
you really appreciated not having to parse an extra set of source
files!)

There's no obvious build procedure but the files 'pdsmon.lod.1' and
'pdsmon.old.1' appear to embed a sequence of commands that created a
host-side executable that embedded the IGM as "data" and pumped it out
to the IMLAC when it was ran. These files show that after the loader
ran (which left a process with the resulting image in memory), the
debugger was invoked to cause the code to eat the character table and
the resulting address space was saved as an executable. "od -c" (or
viewing with EMACS) might be needed as some of the characters don't
easily print otherwise. It's very inelegant, for example the SSAVE
command used in the ".old" file enumerates the page ranges it wanted
that were specific to what DDT, the loader, and the data for IGM
occupied in the address space.
