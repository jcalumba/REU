import sys
import random
import time
start_time = time.time()

def isCon(nodeIndex, node1, node2):
	if node2 in nodeIndex[node1]:
		return True
	else:
		return False

def getID(signature):
	if signature == [2, 1, 0, 0]:
		return 1	
	elif signature == [0, 3, 0, 0]:
		return 2
	elif signature == [2, 2, 0, 0]:
		return 3
	elif signature == [3, 0, 1, 0]:	
		return 4
	elif signature == [0, 4, 0, 0]:
		return 5
	elif signature == [1, 2, 1, 0]:
		return 6
	elif signature == [0, 2, 2, 0]:
		return 7
	elif signature == [0, 0, 4, 0]:
		return 8
	elif signature == [2, 3, 0, 0]:
		return 9
	elif signature == [3, 1, 1, 0]:
		return 10
	elif signature == [4, 0, 0, 1]:
		return 11
	elif signature == [2, 1, 2, 0]:
		return 12
#	elif signature == [1, 3, 1, 0]:
#		return error				# graphlet 13
	elif signature == [2, 2, 0, 1]:
		return 14
	elif signature == [0, 5, 0, 0]:
		return 15
#	elif signature == []:
#		return 16				# pair no graphlet 16
	elif signature == [1, 2, 1, 1]:
		return 17
	elif signature == [0, 4, 0, 1]:
		return 18
	elif signature == [1, 1, 3, 0]:
		return 19
#	elif signature == [0, 3, 2, 0]:			#pair no 2
#		return 20
#	elif signature == [0, 3, 2, 0]:			#pair no 2
#		return 21
	elif signature == [0, 3, 0, 2]:
		return 22
	elif signature == [1, 0, 3, 1]:
		return 23
	elif signature == [0, 2, 2, 1]:
		return 24
	elif signature == [0, 1, 4, 0]:
		return 25
	elif signature == [0, 1, 2, 2]:
		return 26
	elif signature == [0, 0, 4, 1]:
		return 27
	elif signature == [0, 0, 2, 3]:
		return 28
	elif signature == [0, 0, 0, 5]:
		return 29

def genSignature(nodeIndex, graphletSought):
	preSig = [0,0,0,0,0]
	postSig = [0,0,0,0]
	graphletSize = len(graphletSought)
	outer = int(0)
	inner = int(1)
	while outer < graphletSize:
		inner = outer
		while inner < graphletSize:
			if isCon(nodeIndex, graphletSought[outer], graphletSought[inner]):
				preSig[outer] += 1
				preSig[inner] += 1
			inner += 1
		outer += 1

	for x in preSig:			#I don't actually remember why this is here
		if x-1 >= 0:			#I think it fixed some weird behavior with signatures returning negative values
			postSig[x-1] +=1

	return postSig

with open (sys.argv[1]) as g:
	chainList = g.read()

chainList = chainList.split('\n')
chainList.pop()
chainList.sort()

chainListParsed = []

for x in chainList:
	chainListParsed.append(x.split('\t')[0])

outputList = [[] for i in chainListParsed]			#this is the list of lists that will hold the final output
i = 0
for x in chainListParsed:
	outputList[i].append(x)
	i += 1

for FILE_COUNT in range(2, len(sys.argv)):

	f = open(sys.argv[FILE_COUNT] + '.ID', 'w') 		#the output file, where the program will print the chain and its respective graphlet ID
	
	with open (sys.argv[FILE_COUNT]) as g:
		lines = g.read()
	g.close()
	lines = lines.split('\n')
	lines.pop()
	lines.sort()

	formatLines = []			#lines distilled into a list with two elements only, per line in list
	for x in lines:
		smallList = []
		smallList = x.split("	")
		smallList[0] = int(smallList[0])
		smallList[1] = int(smallList[1])
		formatLines.append(smallList)	#this will yield a set of sets which contain two nodes connected by an edge

	formatLines.sort()

	largestNode = 0				#fetch the largest index of a node, needed for the graph builder
	for x in formatLines:
		if int(x[0]) > largestNode:
			largestNode = int(x[0])
		if int(x[1]) > largestNode:
			largestNode = int(x[1])

	largestNodePlus = largestNode + 1

	nodeIndex = [[] for i in range(largestNodePlus)]	#a list of lists the size of the number of nodes in the graph
							#the sub lists will hold the connections for the node of the sublist's index

	for x in range(0, largestNodePlus):  			 #creating a list of nodes and their connections
		for y in formatLines:	 		  		 #iterating over every line to check to see if it's applciable
			index = int(y[0])
			if index == x and y[1] not in nodeIndex[x]: 
				nodeIndex[x].append(y[1])
				nodeIndex[y[1]].append(index)			


	asdf = open(sys.argv[FILE_COUNT] + ".nodeindex", 'w')
	for x in range(0, len(nodeIndex)):
		asdf.write(str(x))
		asdf.write( ":")
		asdf.write(str(nodeIndex[x]))
		asdf.write("\n")
	asdf.close

	sigCount = 0

	for i in chainListParsed:
		sigCount += 1
		graphletSought = i.split("_")			#defining the Markov Chain to be tested
		graphletSought.pop()
		graphletSought = list(map(int, graphletSought))		#converting the strings to int
	
		foo = genSignature(nodeIndex, graphletSought)
		
		if foo != [1, 3, 1, 0] and foo != [0, 3, 2, 0]:
			if sum(foo) == len(graphletSought):
				f.write(str(getID(foo)))
				outputList[sigCount-1].append(getID(foo))
			else:
				f.write('NONE')
				outputList[sigCount-1].append('NONE')
		else:
			preIsolatedGraphlet = [0, 0, 0, 0, 0]
			for x in range(0, 5):
				for y in range(0, 5):
					if isCon(nodeIndex, graphletSought[x], graphletSought[y]):
						preIsolatedGraphlet[x] += 1
						preIsolatedGraphlet[y] += 1
			nodeDeg1 = "error"
			nodeDeg3 = "error"
			nodeDeg3_2 = "error"
			for x in range(0, 5):
				if preIsolatedGraphlet[x] == 2:
					nodeDeg1 = int(graphletSought[x])
				if preIsolatedGraphlet[x] == 6 and nodeDeg3 == "error":
					nodeDeg3 = int(graphletSought[x])
				if preIsolatedGraphlet[x] == 6 and int(graphletSought[x]) != nodeDeg3:
					nodeDeg3_2 = int(graphletSought[x]) 
			if foo == [1, 3, 1, 0]:
				if isCon(nodeIndex, nodeDeg1, nodeDeg3):
					f.write("16")
					outputList[sigCount-1].append(16)
				else:
					f.write("13")
					outputList[sigCount-1].append(13)
			elif foo == [0, 3, 2, 0]:
				if isCon(nodeIndex, nodeDeg3, nodeDeg3_2):
					f.write("21")
					outputList[sigCount-1].append(21)
				else:
					f.write("20")
					outputList[sigCount-1].append(20)
	
		f.write("	")
	
		for y in graphletSought:
			f.write(str(y))
			f.write("_")
		f.write("\n")


f = open(sys.argv[1] + ".final", 'w')
f.write("Walk \t\t")
for x in range(2, len(sys.argv)):
	f.write("Time ")
	f.write(str(x - 1))
	f.write("\t")
f.write('\n')
for x in outputList:
	for y in x:
		f.write(str(y))
		if len(str(y)) == 6:
			f.write('\t')
		f.write('\t')
	f.write('\n')

#ANALYSIS

transOpportunities = [0 for x in range(0, 29)]
transCount = [0 for x in range(0, 29)]
transMatrix = [[0 for x in range(0, 29)] for x in range (0, 29)]

#transMatrix is a 29 by 29 matrix where the row corresponds to a base graphlet and the column represents a transition
#from the base to the column. So if the location (1, 2) has the value of 3, that means that three separate instances of 
#graphlet 1 transition to graphlet 2, for a total of three transitions from 1 to 2

for x in outputList:
	for y in range(1, len(x) - 1):
		if x[y] != "None" and x[y] != "NONE" and x[y + 1] != "None" and x[y + 1] != "NONE" and x[y+1]:

			transOpportunities[x[y] - 1] += 1   		    #I subtract one from x[y] because I want the 29 slots in Opportunities to 
			if x[y] != x[y+1]:				    #correspond to the twenty nine graphlets (so 1-29 maps to 0-28)	
				transCount[x[y] - 1] += 1
				transMatrix[x[y] -1][x[y+1]-1] += 1

f.write("\nGraphlet: \t Transition Probability \n")
for x in range(0, 29):
	if transOpportunities[x] > 0:
		f.write('\t')
		f.write(str(x+1))
		f.write(": ")
		f.write('\t')
		f.write("{:.2%}".format((float(transCount[x]) / transOpportunities[x])))
		f.write('\n')
	else:
		f.write('\t')
		f.write(str(x+1))
		f.write(": ")
		f.write('\t')
		f.write("0.0")
		f.write('\n')

print("%s seconds" %(time.time()-start_time))

for x in transMatrix:
	f.write(str(x))
	f.write('\n')
