import random
import numpy as np
DATA = [[3, 7],# Benefit, Weight
        [8, 8],
        [3, 4],
        [2, 10],
        [7, 4],
        [9, 6],
        [4, 4]]
INDIVIDUAL_NAMES = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']


######################## CREATING CLASS

class Individual:
	def __init__(self, name, encoding):
		self.name = name
		self.encoding = encoding          # This is a list with len = 7 (no of genes)
		self.weight = self.getWeight()
		self.fitness = self.getFitness()
		self.probability = 0
		self.cumProb = 0
	
	def getWeight(self):
		weightTemp = 0
		for i in range(len(self.encoding)):
			if self.encoding[i] == 1:
				weightTemp += DATA[i][1]
		return weightTemp
	
	def getFitness(self):
		fitnessTemp = 0
		for i in range(len(self.encoding)):
			if self.encoding[i] == 1:
				fitnessTemp += DATA[i][0]
		if self.weight <= 22:
			return fitnessTemp
		else:
			return 0
			
	def getEncoding(self):
		return self.encoding
		
	def setProbability(self, fitness, fitnessCumulative):
		self.probability = fitness/fitnessCumulative
	
	def setCumProb(self, cumProb):
		self.cumProb = cumProb


MAX_LOOP = 10
initialPop = True
totalFitnesses = []

for gen in range(MAX_LOOP):

	print('#################### GENERATION NO', gen, '####################')
	print()

	print('### New individuals gen', gen, '###')
	######################## 
	# Create either 1 or 0 as the genes of each Individuals.
	INDIVIDUALS = [None for _ in range(len(INDIVIDUAL_NAMES))]
	if initialPop == True:
		random.seed(13)
		for i in range(len(INDIVIDUALS)):
			INDIVIDUALS[i] = Individual(INDIVIDUAL_NAMES[i], [random.randrange(2) for _ in range(len(DATA))])			# Genes nya random
			print(INDIVIDUALS[i].name, INDIVIDUALS[i].encoding, 
				INDIVIDUALS[i].weight, INDIVIDUALS[i].fitness)
		print()
		initialPop = False

	else:
		INDIVIDUALS = [None for _ in range(len(INDIVIDUAL_NAMES))]
		for i in range(len(INDIVIDUALS)):
			INDIVIDUALS[i] = Individual(INDIVIDUAL_NAMES[i], ENCODINGS[i])
			print(INDIVIDUALS[i].name, INDIVIDUALS[i].encoding, 
				INDIVIDUALS[i].weight, INDIVIDUALS[i].fitness)
		print()


	######################## 
	# Sort the Individuals starting from the highest fitness.

	print('### Sorted individuals gen', gen, '###')
	INDIVIDUALS.sort(key=lambda x: x.fitness, reverse=True)

	for i in range(len(INDIVIDUALS)):
		print(INDIVIDUALS[i].name, INDIVIDUALS[i].encoding, 
			INDIVIDUALS[i].weight, INDIVIDUALS[i].fitness)

	print()

	######################## 
	# Calculating fitnessCumulative and the probability of each Individual.

	print('### Probability of individuals gen', gen, '###')
	fitnessCumulative = 0
	for i in range(len(INDIVIDUALS)):
		fitnessCumulative += INDIVIDUALS[i].fitness

	for i in range(len(INDIVIDUALS)):
		INDIVIDUALS[i].setProbability(INDIVIDUALS[i].fitness, fitnessCumulative)           # Cari probabilitas dari tiap individual

	for i in range(len(INDIVIDUALS)):
		print(INDIVIDUALS[i].name, INDIVIDUALS[i].encoding, 
			INDIVIDUALS[i].weight, INDIVIDUALS[i].fitness, INDIVIDUALS[i].probability)
	totalFitnesses.append(fitnessCumulative)
	print()

	######################## 
	# Calculating cumProb of each Individual.
	
	print('### Cumulative probability of individuals gen', gen, '###')
	cumProb = 0
	for i in range(len(INDIVIDUALS)):          # Ngitung cumulative probability dari tiap individual
		cumProb += INDIVIDUALS[i].probability
		INDIVIDUALS[i].setCumProb(cumProb)
		
	for i in range(len(INDIVIDUALS)):
		print(INDIVIDUALS[i].name, INDIVIDUALS[i].cumProb)

	print()

	######################## 
	# Creating random values to perform RWS selection. The random values are stored in randomRWS list.

	random.seed(13)
	randomRWS = []
	for i in range(len(INDIVIDUALS)):          # Ngebuat random values untuk RWS
		randomRWS.append(random.random())

	print('### Generated random values to perform RWS selection gen', gen, '###')
	for i in range(len(randomRWS)):        # The length of randomRWS is the same as INDIVIDUALS length
		print(randomRWS[i])

	print()

	########################
	# Selection process goes below. INDIVIDUALS list stores individuals along with its probability. resultRWS list stores the chosen individuals.

	resultRWS = []          # Empty list to store selected Individuals
	for i in range(len(randomRWS)):
		for j in range(len(INDIVIDUALS)):
			if randomRWS[i] < INDIVIDUALS[j].cumProb:
				resultRWS.append(INDIVIDUALS[j])          # Selection process using RWS
				break
			else:
				continue

	print('### Selected individuals gen', gen, '###')
	for i in range(len(resultRWS)):
		print(resultRWS[i].name, resultRWS[i].fitness, resultRWS[i].encoding)

	print()

	########################
	# Sort the selected Individuals according to its fitness.

	resultRWS.sort(key=lambda x: x.fitness, reverse=True)          # Nge sort nya dari yg fitnessnya tinggi dulu

	print('### Sorted selected individuals based on fitness gen', gen, '(the two best individuals will not be crossovered)###')
	for i in range(len(resultRWS)):
		print(resultRWS[i].name, resultRWS[i].fitness, resultRWS[i].encoding)

	print()

	########################
	# Keep two best Individual for elitism while shuffling the rest.

	resultRWScopy = resultRWS[2:]          # Keep two best individual at the top of the list, so the two individuals will not be crossovered.
	random.shuffle(resultRWScopy)
	resultRWS[2:] = resultRWScopy


	########################
	# Multipoint crossover goes below.
	print('### Multipoint crossover (index 0, 2, 4, 6) gen', gen, '###')
	ENCODINGS = [resultRWS[i].encoding for i in range(2)]

	CROSSOVER = [resultRWS[i].encoding for i in range(2,len(resultRWS[0].encoding)+1)]
	CROSSOVER = np.array(CROSSOVER)
	CROSSOVER = CROSSOVER.tolist()


	print("Before crossover")
	for i in range(len(CROSSOVER)):
		print(CROSSOVER[i])
	print()

	for i in range(0,len(CROSSOVER),2):
		for j in range(0,len(CROSSOVER[0]),2):
			temp = CROSSOVER[i][j]
			CROSSOVER[i][j] = CROSSOVER[i+1][j]
			CROSSOVER[i+1][j] = temp

	print("After crossover")
	for i in range(len(CROSSOVER)):
		print(CROSSOVER[i])
	print()

	########################
	# Join the list ENCODINGS (initially only contains the two best encodings) with CROSSOVER
	ENCODINGS += CROSSOVER
	print('### Elitism individuals + crossovered individuals gen', gen, '###')
	for i in range(len(ENCODINGS)):
		print(ENCODINGS[i])
	print()


print('### Summary ###')
print('Total fitness of each generation')
for i in range(len(totalFitnesses)):
	print('Gen', i, ':', totalFitnesses[i])

'''
Gen 0 : 60
Gen 1 : 101
Gen 2 : 191
Gen 3 : 213
Gen 4 : 224
'''
