Algorithm 1. The Jaya algorithm for graph layout.
Algorithm Jaya (G, populationSize, iterations)
Input: Graph G, populationSize, iterations
Output: Best candidate
1: Initialize population of candidate graph layouts with random positions
2: Evaluate cost of each candidate
3: Find best and worst candidates
4: For I from 1 to iterations do
5: generateNewPopulation(population[], best, worst)
6: Update best and worst candidates in population
7: End For
8: Return best candidate

Algorithm 2. Generate new population algorithm.
Algorithm generateNewPopulation(population[],bestCandidate, worstCandidate)
Input: population[], bestCandidate, worstCandidate
Output: population[]
1: For i from 1 to populationSize do
2: updateCandidateJaya(i, bestCandidate, worstCandidate)
3: Evaluate cost of candidate
4: If candidate.cost < population[i].cost then
5: population[i] = candidate
6: End If
7: End For
8: Return population[]

Algorithm 3. Update candidate algorithm.
Algorithm updateCandidateJaya(candidate, bestCandidate,
worstCandidate)
Input: candidate, bestCandidate, worstCandidate
Output: candidate
1: For each node in candidate do
2: xNew = candidate[node].x + rand() _ (bestCandidate[node].x − abs (candidate[node].x)) − rand() _ (worstCandidate[node].x − abs(candi- date[node].x))
3: yNew = candidate[node].y + rand() _ (bestCandidate[node].y − abs (candidate[node].y)) − rand() _ (worstCandidate[node].y − abs(candi- date[node].y))
4: candidate[node].x = xNew
5: candidate[node].y = yNew
6: End For
7 Return candidate

### metrics for a graph:

1. the distribution of nodes
   - We measure the distribution of nodes across the drawing space by maximizing the distances between nodes.
   - The objective of uniformly distributing the nodes across the drawing space necessitates maximizing the distances between nodes.
   - ![[Screenshot 2024-09-19 at 7.36.42 AM.png]]
   - where dij represents the Euclidean distance between two nodes i and j, and i!=j.
2. how similar the edge lengths are
   - Similar edge lengths are measured by specifying a target length and finding a sum of how far each edge is from this length.
   - Equalizing the edge lengths involves designating a specific length (len), followed by adjusting all edges to conform to this predetermined length.
   - ![[Screenshot 2024-09-19 at 7.37.34 AM.png]]
   - where e is an edge and len is the desired length
3. edge crossings
   - We count edge crossings, aiming to decrease that number in each optimization iteration.
   - For the minimization of crossing edges, our approach primarily involved identifying the number of intersecting edges, and endeavoring to reduce this count throughout each iteration of the optimization process.
4. angular resolution
   - we improve angular resolution by measuring the distance between adjacent edges connected to a node and giving high values to small angles.
   - we determined this by amplifying the separation between incident edges, accomplished through the application of the subsequent
   - ![[Screenshot 2024-09-19 at 7.38.31 AM.png]]
   - where deg(v) denotes the degree of a node v
   - theta(e1, e2) is the angle in radians between 2 adjacent edges e1,e2 incident to node v

![[Screenshot 2024-09-19 at 7.40.28 AM.png]]
where w_i and m_i are the weight and measure for the criterion respectively

notes about weights and measures:

- We introduced a normalization process to ensure each measure’s value ranges between 0 and 1 to avoid one measure overpowering the others.
- We assign equal weights to all criteria since it was challenging to establish universal weights that perform well for all graph sizes.
  In any case the values for weights will depend on application area and user preference.
