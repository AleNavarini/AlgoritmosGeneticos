# AlgoritmosGeneticos
import random
import decimal

#Definiton Class

class Cromosoma:

    def __init__(self):
	    self.id = -1
	    self.binary = ""
	    self.decimal = -1
	    self.fitness = -1

    def getFunctionObjective(self):
        return self.decimal ** 2

    def setFitness(self, sumatoryFuncObj):
        self.fitness = round(self.getFunctionObjective() / sumatoryFuncObj, 2)

#End Definiton Class

#Definiton Functions

#Of internet, unknown operation
def ConvertToBinary(d_num):
    assert d_num >= 0

    if d_num == 0:
        return '0'
    b_num = ""

    while d_num > 0:
        b_num = str(d_num % 2) + b_num
        d_num = d_num // 2
    return b_num

#Returns max value fitness of the array
def getMaxFitness(arrayCromosomas):
    valueMax = -1
    for x in range(0, len(arrayCromosomas)):
        if arrayCromosomas[x].fitness > valueMax:
            valueMax = arrayCromosomas[x].fitness

    return valueMax

#Returns max value object of the array
def getMaxObject(arrayCromosomas):
    valueMax = -1
    for x in range(0, len(arrayCromosomas)):
        if arrayCromosomas[x].getFunctionObjective() > valueMax:
            valueMax = arrayCromosomas[x].getFunctionObjective()

    return valueMax

def loadRoulette(arrayCromosomas):
    roulette = []

    for x in range(0,4):
        for j in range(0, int(arrayCromosomas[x].fitness * 100)):
            roulette.append(arrayCromosomas[x].id)

    return roulette

def getCromosoma(id, cromosomas):
    for cromosoma in cromosomas:
        if cromosoma.id == id:
            return cromosoma

def crossover(cromoOne, cromoTwo, posCut, arraySons):
    sonOne = Cromosoma()
    sonTwo = Cromosoma()

    sonOne.binary = cromoOne.binary[:posCut]
    sonTwo.binary = cromoTwo.binary[:posCut]

    if(posCut == 0 or posCut == 5):
        arraySons.append(sonOne)
        arraySons.append(sonTwo)
        return

    for x in range(posCut, len(cromoOne.binary)):
        sonOne.binary += cromoTwo.binary[x]
        sonTwo.binary += cromoOne.binary[x]

    arraySons.append(sonOne)
    arraySons.append(sonTwo)

def Main():
    #Definitions vars

    initialPoblation = []
    fatherPoblation = []
    sonPoblation = []
    sumatoryFuncObj = 0
    sumatoryFitness = 0
    probMut = 0.001
    probCross = 0.9
    roulette = []

    #End Definitions vars


    #put 4 cromosomas in the initial poblation
    for x in range(0,4):
        cromosoma = Cromosoma() #Create a instance of class Cromosoma

        cromosoma.id = x + 1
        cromosoma.decimal = random.randint(0,31) #Generate ramdom number for represent cromosoma
        cromosoma.binary = ConvertToBinary(cromosoma.decimal).rjust(5, '0') #Convert decimal number ramdom to binary (rjust put chars left of a string)
        sumatoryFuncObj += cromosoma.getFunctionObjective()

        initialPoblation.append(cromosoma) #Append cromosoma in initial poblation

    #Set value fitness to Cromosomas
    for x in range(0,4):
        initialPoblation[x].setFitness(sumatoryFuncObj)
        sumatoryFitness += initialPoblation[x].fitness

    print('ID','Cromosoma','Decimal','FunObjetivo','Fitness')
    for cromosoma in initialPoblation:
        if cromosoma.decimal>9 :
            print(cromosoma.id,' ',cromosoma.binary,'    ',cromosoma.decimal,'     ',str(cromosoma.getFunctionObjective()).rjust(3,'0'),'     ',cromosoma.fitness)
        else:
            print(cromosoma.id,' ',cromosoma.binary,'     0'+str(cromosoma.decimal),'     ',str(cromosoma.getFunctionObjective()).rjust(3,'0'),'     ',cromosoma.fitness)


    #print("Suma: ", sumatoryFuncObj, sumatoryFitness)
    #print("Promedio: ", sumatoryFuncObj / len(initialPoblation), sumatoryFitness / len(initialPoblation))
    #print("Maximo: ", getMaxObject(initialPoblation), getMaxFitness(initialPoblation))


    #Roulette
    roulette = loadRoulette(initialPoblation)

    for x in range(0,4):
        valRandom = random.randint(0,99)
        fatherPoblation.append(getCromosoma(roulette[valRandom], initialPoblation))

    for x in range(0, 2):
        if x == 1:
            x=x+1
        fatherOne = fatherPoblation[x]
        fatherTwo = fatherPoblation[x + 1]
        print(x)
        print(x + 1)







  #  isCrossover = random.uniform(0, 1)

    #if isCrossover < probCross:
    #   posCut = random.randint(0, 5)
    #else:

    #isMutation = random.uniform(0, 1)
#
#    #if isMutation < probMut:
#    #    print("Hacemos Mutacion")
#

#    print(sonPoblation)

Main()
