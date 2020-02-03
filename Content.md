# STAIRS BET

# CHALLENGE
You want to bet a friend that you can go up at least 60 steps under the following rules:
  1. You roll a dice 100 times. 
  2. If you roll 1 or 2 you go down one step. 
  3. If you roll a 3,4,5 you go up one step. 
  4. If you roll a 6, you roll again and take that many steps
  5. There is a 0.1% chance each time you roll the dice that you will fall down the stairs

# SOLUTION (described): 
We are going to simulate the bet 250 times and create a distribution of the final step reached on each try (=random walk).

# CODE

    #import libraries
    import numpy as np
    import matplotlib.pyplot as plt
    np.random.seed(100) #we set a constant seed so we can control as we write the code.

    #Simulate random walk 250 times
    all_walks = []
    for i in range(250):
        #Simulate one random walk with 100 dice rolls
        random_walk = [0]
        for j in range(100):
            step = random_walk[-1]    #keeps track of position throughout all iterations
            dice = np.random.randint(1,7)     #randomised dice
            
            #apply rules
            if dice <= 2:
                step = max(0, step - 1) #doesn't fall below step 0
            elif dice <= 5:
                step = step + 1
            else:
                step = step + np.random.randint(1,7)
            if np.random.rand() <= 0.001 : #0.1% chance of falling down the stairs
                step = 0

            random_walk.append(step) #last item in list is current location. Keeps track of positions as the bet goes on
        
        all_walks.append(random_walk) 

    #Create and plot all random_walks - Not insightful but we can visualise what's going on
    np_aw = np.array(all_walks)
    np_aw_t = np.transpose(np_aw) #transpose is needed - computer will plot the collumns not the rows of the matrix

    plt.plot(np_aw_t)
    plt.show()
    plt.clf()

    # Select last row from np_aw_t: final positions for each walk
    final_p = np_aw_t[-1,:]
    # Plot histogram of ends, displaying the distribution of our final position had we accepted the bet
    plt.hist(final_p, bins=10)
    plt.show()

    success=len(final_p[np.array(final_p>=60)]) #No. of times that we ended above 60 after 250 simulations
    print(str(success/250) + "% probability of winning the bet")
    
# Outcome: 
This code will provide us with a distribution of the final position of this simulation. Thus giving us an image of what to expect. The last two lines give us the probability of success.
    
