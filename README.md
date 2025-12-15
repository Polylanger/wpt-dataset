# WPT-dataset

Datasets used in the "Web Page Tampering Detection Based on Dynamic Temporal Graph Pre-training"

## Collection

We maintain a list of 600 authorized websites for collection, mainly focused on industries such as finance, healthcare, and education. Through a two-month scheduled collection task, we obtained an initial dataset containing 21 snapshots and more than 3 million normal web pages. We crawl authorized domains with per-host throttling (QPS ≤ 50 by default), respect robots.txt (including crawl-delay). Timeouts are set to 10 s with up to 3 retries on transient errors. A new time slice starts every 24 h at 02:00 (UTC+8) and the crawl lasts ≈5 h.

## Composition

We collected real web page tampering samples from zone-h.org and integrated them into our dataset in proportion to different categories of tampering. The ratio of normal samples to abnormal samples is shown in the table below:

|   |   |   |   |   |
|---|---|---|---|---|
|Tampering Type|Number of Snapshots|Number of Edges|Number of Benign Nodes|Number of Positive Nodes|
|Normal web page|1352|3110317|179143|0|
|Replace homepage|54|169621|7734|54|
|Implant trojan|57|148535|9475|57|
|Malvertising inserted|68|165016|7653|1430|

For the tampering of pages through "Implant trojan" and "Malvertising inserted", we have set the following restrictions: token editing not exceeding 30%, no more than 5 new external links, no more than 3 new scripts (without deleting existing scripts), and no more than a 5% increase in DOM nodes. 

For homepage tampering, we download the altered homepages from zone-h.org, such as: [link1](http://zonehmirrors.org/defaced/2023/06/19/camaraseverinia.sp.gov.br/camaraseverinia.sp.gov.br/), [link2](http://zonehmirrors.org/defaced/2023/06/20/geyve.bel.tr/geyve.bel.tr/), [link3](http://zonehmirrors.org/defaced/2023/06/16/naran.su.gov.mn/naran.su.gov.mn/), and directly replace the homepage content.

For "Implant trojan", we collect web trojan samples from zone-h.org and the internet, extracting the malicious parts and embedding them into the corresponding pages. For "Malvertising inserted", we gather real malicious ad snippets from the internet, extracting the ad portions and embedding them into the pages.

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

## Cite

```bib
@article{xu2025web,
  title={Web Page Tampering Detection Based on Dynamic Temporal Graph Pre-training},
  author={Xu, Yijia and Zhang, Qiang and Wang, Kaiyang and Liu, Zhonglin and Huang, Cheng and Fang, Yong},
  journal={IEEE Transactions on Dependable and Secure Computing},
  year={2025},
  publisher={IEEE}
}
```
