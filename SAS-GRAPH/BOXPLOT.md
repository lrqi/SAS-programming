## PROC SGPLOT
*http://support.sas.com/documentation/cdl/en/grstatproc/65235/HTML/default/viewer.htm#n1waawwbez01ppn15dn9ehmxzihf.htm

ods listing style=journal;
ods graphics / width=6.5in height=5in border=off;

%GET_TF;
  title%eval(&title_count.+2) j=c "Study Eye: #byval1";
  title%eval(&title_count.+3) j=c "Time Relative to Dosing: #byval2 hours";
  title%eval(&title_count.+4) j=c "MDTP = #byval4";

proc sgplot data=final1;
  /*  generate a vertical box plot that groups by DOSEN*/
  by studyeye atptn  trtapen TRTPG1;
  vbox aval / category=DOSEN group=ady groupdisplay=cluster
  lineattrs=(pattern=solid) whiskerattrs=(pattern=solid) EXTREME MEANATTRS=(symbol=circle) ; 
  xaxis display=all label=" " ;
  yaxis grid label='%IOP Reduction from Baseline (ng/mL)';
  keylegend / across=2;
run;

