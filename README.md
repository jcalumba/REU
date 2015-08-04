# REU
Graphlet Analysis and Change Prediction tool

This is a tool designed to read in one input file of walks to read and identify as graphlets 1 through 29, and an arbitrary number of timestamp input files. The program will identify every walk on every timestamp. There are three outputs, one file called VISITFILENAME.final, which contains a list of walks and their identities per given time, plus statistics based on how each graphlet transitioned into another over time. The final block at the end of the *.final file is a list of 29 walks with 29 values in each list, which represents the number of times the outer number (the walk) changed into the inner number. So if the value at (1,3) is 2, that means that there were two instances of G1 changing to G3. The other two output files are creatd per input time snapshot, one SNAPSHOT.ID file, which contains just the walks identified at that snapshot's time, and the SNAPSHOT.nodeindex, which contains a list of lists that is how the program represents the graph at time SNAPSHOT. The program itself is supposed to be segmented and easily modifiable, hence the #ANALYSIS section at the end.

usage:
  ./analysis_tool.py VISITFILE TIME1 TIME2...
  
  Where VISITFILE is the list of walks to be tested over the various graph snapshots, represented as TIME1, TIME2, etc.
  Examples for the format of each file are available on at github.com/jcalumba/REU 
  
  Visitfile contains a list of walks represented as nodes deliminated by '_', and walks separated by newlines(\n).
  TIME1 TIME2 contain graph snapshots represented as a list of connections which are represented as "(node1) \t         (node2)"
  that is, for example  
    1   2
  or 1 connects to 2
  
  The program assumes that connections are bidrectional, that nodes cannot connect to themselves, and that nodes have    at most one connection to any other node.
    
    
