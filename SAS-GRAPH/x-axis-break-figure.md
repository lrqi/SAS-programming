## use annotation to do the break and annotate for x label: ***;

data anno;
  set nsub;
	retain  xsys '2' ysys '3' hsys '3' 
		      cbox 'white' size 3  when 'a'
			    function 'label' 
		      position '5';   **annotation options;
  text='Week 1';
  x=0; y=8;
  output;
  text='//';
  x=0; y=17;      **y means height from figure bottom, 17%; x is equal to x-axis value
  output;
  ...
run;

*** Set format for display of x-axis ***;
data ctrl; 
	retain fmtname 'myFormat' type 'n';    
	do time=0 to 72 by 8;    
	start=time-0.3; end=time+0.3;
	if time>=24 then z=time-24;
	else if time in (0,8,16) then z=0;
	else z=.;
	if z>.z then label=put(z,10.0);   
	else label=''; 
	output;   
	end;   
run;

proc format library=work cntlin=ctrl;  
run;

*** Generate linear plot ***;
options spool nobyline orientation=portrait;
goptions reset=all device=sasemf  hsize=6.5in vsize=3.6in htext=1 ftext=complex ftitle=complex noborder;
symbol1 c=black h=1 i=join value=circle repeat=256;

proc gplot data=mplot gout=&fig;
		by subjid;
		title1 j=c height=1 "Subject: #byval(subjid)"; 	
		title2 j=c height=1 "Linear Scale";
    plot aval*time=text / vaxis=axis2 haxis=axis1 anno=anno nolegend noframe;
		format time myFormat.;
run;


![x-axis break figure](https://user-images.githubusercontent.com/28472272/48308708-865d7080-e538-11e8-849e-6a250236bbf3.PNG)
