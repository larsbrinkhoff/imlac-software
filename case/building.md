## BUILD INSTRUCTIONS

Add a file UNIV.MAC with the contents `%UNIV%==1`.  Add a file END.MAC
with the contents `END`.

Assemble with MACRO: `PDSDEF=UNIV,PDSDEF,END`

Assemble with MACRO: `LOADER=LOADER`

Assemble with MACRO: `NPDSDM=NPDSDM`

Edit the IGM.MAC file:
- Change the MOVEI macro to just output `LAC [A]`
- Change `REPI WCURS,DJMS ECUR` to `REPI WCURS,DJMS. ECUR`

Assemble with MACRO: `IGM=IGM`

