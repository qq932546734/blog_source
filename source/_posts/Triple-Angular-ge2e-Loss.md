---
title: 'Triple, Angular, ge2e Loss'
date: 2019-07-24 14:14:35
tags:
---


## Triple loss

### mini batch selection
Select *M* people, each people several items (there is no need to be equal for all people). For example, each batch select 10 person, each person got 20 face images / voice segments.

### generate triplet
Every items will be paired with items from the same people to generate positive pairs. For each positive pair, calculate the distances between the anchor item and items from different owner `negative_distances`. By subtructing `positive_distances` (positive pair distance) from `negative_distances`, we can have all negative pairs that meet our requirement: `negative_distance` - `positive_distance` is less than `alpha`, which means the negative item are too close to anchor item compared with positive item. Then from these negative pairs, select one randomly to generate triplet. Till now, we got a list of triplets: [`anchor item`, `positive item`, `negative item` ]

### calcuate loss
Cossin similarity or Squared distance could be used to calculate the loss.

```python

def select_triplets(embeddings, nrof_images_per_class, image_paths, people_per_batch, alpha):
    """ Select the triplets for training
    """
    trip_idx = 0
    emb_start_idx = 0
    num_trips = 0
    triplets = []
    
    # VGG Face: Choosing good triplets is crucial and should strike a balance between
    #  selecting informative (i.e. challenging) examples and swamping training with examples that
    #  are too hard. This is achieved by extending each pair (a, p) to a triplet (a, p, n) by sampling
    #  the image n at random, but only between the ones that violate the triplet loss margin. The
    #  latter is a form of hard-negative mining, but it is not as aggressive (and much cheaper) than
    #  choosing the maximally violating example, as often done in structured output learning.

    for i in xrange(people_per_batch):  # iterate over each person
        nrof_images = int(nrof_images_per_class[i]) # How many images each person got
        for j in xrange(1,nrof_images): # iterate over all images belonging to the current person
            a_idx = emb_start_idx + j - 1
            neg_dists_sqr = np.sum(np.square(embeddings[a_idx] - embeddings), 1)
            for pair in xrange(j, nrof_images): # For every possible positive pair.
                p_idx = emb_start_idx + pair
                pos_dist_sqr = np.sum(np.square(embeddings[a_idx]-embeddings[p_idx]))
                # exclude positive pairs
                neg_dists_sqr[emb_start_idx:emb_start_idx+nrof_images] = np.NaN 
            
                all_neg = np.where(neg_dists_sqr-pos_dist_sqr<alpha)[0] # VGG Face selecction
                nrof_random_negs = all_neg.shape[0]
                if nrof_random_negs>0:
                    rnd_idx = np.random.randint(nrof_random_negs)   # after getting lots of candidates, choose one randomly
                    n_idx = all_neg[rnd_idx]
                    triplets.append((image_paths[a_idx], image_paths[p_idx], image_paths[n_idx]))
                    #print('Triplet %d: (%d, %d, %d), pos_dist=%2.6f, neg_dist=%2.6f (%d, %d, %d, %d, %d)' % 
                    trip_idx += 1

                num_trips += 1

        emb_start_idx += nrof_images

    np.random.shuffle(triplets)
    return triplets, num_trips, len(triplets)



def loss(y_pred):
    """
    y_pred: [batch_size, embedding_size], it's composed of three parts: 
            first is the anchors' embeddings, second is the positives' embeddings,
            the last is the negative embeddings.
    """

    elements = int(K.int_shape(y_pred)[0] / 3)
    anchor = y_pred[0:elements]

    positive_ex = y_pred[elements:2 * elements]

    negative_ex = y_pred[2*elements:]

    sap = batch_cosin_similarity(anchor, positive_ex)
    san = batch_cosin_similarity(anchor, negative_ex)
    loss = K.maximum(san-sap+alpha, 0.0)
    total_loss = K.sum(loss)

    return total_loss
```