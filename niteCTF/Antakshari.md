# Description
My friend can never remember movie names, absolutely hopeless. This time he only recalls six cast members, nothing else. He's always been dense, but we've been a tight for years, so I guess I'm stuck helping him again. Can you figure out the movie he's trying to remember?
# Solution
- 6 cast members denote the 6 nodes that are clustered together that we need to find in the neural network, which then have to be arranged in descending order to get the flag.
- To find the cluster of nodes we can use the K Nearest Neighbors Classification.
- K-Nearest Neighbors Classifier first stores the training examples. During prediction, when it encounters a new instance (or test example) to predict, it finds the K number of training instances nearest to this new instance.  Then assigns the most common class among the K-Nearest training instances to this test instance.
- Using the nearest neighbours code to find 5 other closest indices to 1 that we choose.
- Similarly, when we get 6 nodes coming together everytime, when nearest distance is measured, gives us the classified cluster we need. Like actors of a single movie would be found together at every testing. Rough snippet of the logic is given below.
```python
import numpy as np
from sklearn.cluster import KMeans

lv = np.load("latent_vectors.npy") #latent vectors embedding

norms = np.linalg.norm(lv, axis=1) #Euclidian length of vectors
labels = KMeans(n_clusters=2, random_state=0).fit(norms.reshape(-1, 1)).labels_ #Forming clusters

movie_label = 1 if norms[labels == 1].mean() > norms[labels == 0].mean() else 0
movies = np.where(labels == movie_label)[0]
actors = np.where(labels != movie_label)[0]

M = lv @ lv.T #Similarity matrix (gives number of items matrix)

for movie in sorted(movies):
    sims = M[movie, actors] #similarity between the movie and each actor
    order = np.argsort(-sims) #most similar sorting
    top6 = actors[order[:6]] #top 6 actors
    seq = ",".join(str(x) for x in sorted(top6, reverse=True)) #sort the 6 actors in reverse order
    print(f"Movie {movie}: seq={seq}")
```
- This prints `Movie 3: seq=189,177,134,108,37,29`
- Submitting it in the url https://antakshari1.chall.nitectf25.live/, we get the flag.
> nite{Diehard_1891771341083729}
