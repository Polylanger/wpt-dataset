# WPT-dataset

Datasets used in the "Web Page Tampering Detection Based on Dynamic Temporal Graph Pre-training"

## Collection

We maintain a list of 600 authorized websites for collection, mainly focused on industries such as finance, healthcare, and education. Through a two-month scheduled collection task, we obtained an initial dataset containing 21 snapshots and more than 3 million normal web pages.

We manually formulated rules to clean the data and finally obtained a batch of data containing more than 210,000 normal web pages collected from 79 websites.

## Composition

We collected real web page tampering samples from zone-h.org and integrated them into our dataset in proportion to different categories of tampering. The ratio of normal samples to abnormal samples is shown in the table below:

|   |   |   |   |   |   |
|---|---|---|---|---|---|
||Tampering Type|Number of Snapshots|Number of Edges|Number of Benign Nodes|Number of Positive Nodes|
|Training Set|Normal Pages|1595|3282874|189861|0|
|Test Set|Website Homepage Replaced|54|169621|7734|54|
||Website Homepage Infected with Trojan|57|148535|9475|57|
||Malicious Ads Inserted in Website|68|165016|7653|1430|

## Feature Extraction

**Structural Features** We parse the webpage structure into tag paths, construct a word bag from the paths, utilize the gensim library to train a doc2vec model on the entire dataset, and generate a 72-dimensional structural feature for each snapshot of the webpage.

**Text Features** We use the Bert model provided by huggingface to convert the text of each webpage into a 768-dimensional word vector, and train an autoencoder to reduce the dimensionality of the word vectors to 64 dimensions.

**Statistical Features** We extract 12 statistical indicators from the webpage content, as shown in Table 2, to form statistical features.

Finally, we concatenate the three types of features to form a 148-dimensional feature vector for each webpage.

## Dataset Format

We use the `save_graphs()` data storage interface provided by [DGL](https://www.dgl.ai/pages/start.html) to save the list of snapshots for each website to disk. The following code example demonstrates how to read a temporal graph of a website from disk.

```python
from dgl import load_graphs

graph_path = 'data/host-1.bin'
graphs = load_graphs(graph_path)
```

For detailed information about the dataset, please refer to the [tutorial.ipynb](tutorials.ipynb) file.

## Credits

The raw temporal-graph data for WPT was provided by the Information Security Institute of Sichuan University.

## Contact

- zhangqiangcs@stu.scu.edu.cn
- xuyijia@stu.scu.edu.cn
