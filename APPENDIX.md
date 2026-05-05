# Appendix

## What the Headlines Say — CS 210 Final Project

Sadhana Vasanthakumar | Spring 2026

---

## A. AI Tool Usage Disclosure

This project used cGPT as a tool in two specific areas, documented here in full for transparency.

**Website development.** The interactive project website (`index.html`) was built with the help of cGPT. The design, color scheme, layout structure, and Chart.js chart configurations were produced through an iterative process with cGPT, starting from a rough description of what the site needed to do. The content throughout the website, including all finding descriptions, figure captions, scanner interpretations, and corpus data tables, reflects the actual analysis from the notebooks and was written and verified by me. cGPT did not produce any of the analytical findings, numbers, or interpretations. It was used to translate those findings into a working HTML/JavaScript file.

**Coding assistance.** Throughout the notebooks, cGPT was consulted for specific implementation questions: correct syntax for DataFrame group-by operations, how to apply MinMaxScaler across columns, how to structure a 5-fold cross-validation loop, how to compute Cohen's kappa with scikit-learn, and similar questions. All conceptual design decisions, metric definitions, formula derivations, and analytical interpretations were my own. cGPT was used the way you would use Stack Overflow: to look up syntax and catch small errors, not to generate the logic or drive the analysis.

**What cGPT was not used for:** the research questions, the definition of PSI, AAI, or the Invisible Human Framework, the lexicon construction, the weighting decisions for AAI components, the hypothesis formulation, the interpretation of findings, the written report, or any statistical analysis.

---

## B. External Resources and Literature Consulted

The following sources informed the design of the analytical frameworks and the interpretation of results. Full citations appear in the report bibliography.

**Media framing and cognitive bias in AI coverage**

- Nguyen, D., and Hekman, E. (2024). The news framing of artificial intelligence: A critical exploration of how media discourses make sense of automation. *AI and Society*, 39(2), 437–451. This paper identified the main framing patterns in AI journalism (surveillance, data bias, information disorder) and was the starting point for thinking about what dimensions to measure.

- Brennen, J. S., Howard, P. N., and Nielsen, R. K. (2018). *An Industry-Led Debate: How UK Media Cover Artificial Intelligence.* Reuters Institute for the Study of Journalism. Used to ground the eight bias dimensions and understand how optimism and fear vocabulary distribute across coverage types.

- Cave, S., and Dihal, K. (2019). Hopes and fears for intelligent machines in fiction and reality. *Nature Machine Intelligence*, 1(2), 74–78. Provided the framework for categorizing AI discourse into utopian and dystopian poles, which directly informed the techno-utopianism and moral panic dimensions.

- Darling, K. (2017). "Who's Johnny?" Anthropomorphic framing in human-robot interaction, integration, and policy. In *Robot Ethics 2.0*. Oxford University Press. The theoretical basis for the anthropomorphism bias dimension and the argument that grammatical framing shapes how readers attribute responsibility.

**Automated bias detection methods**

- Rodrigo-Gines, F.-J., Carrillo-de-Albornoz, J., and Plaza, L. (2024). A systematic review on media bias detection. *Expert Systems with Applications*, 237, 121641. Reviewed to understand the state of automated bias detection before deciding on the hybrid embedding plus keyword approach.

- Castillo-Campos, M., Becerra-Alonso, D., and Boomgaarden, H. (2025). Automated detection of media bias using AI and NLP. *Social Science Computer Review*. Read alongside Rodrigo-Gines to understand why no standard benchmark existed and what that meant for validation design.

**NLP and machine learning methods**

- Hutto, C. J., and Gilbert, E. (2014). VADER: A parsimonious rule-based model for sentiment analysis of social media text. *Proceedings of AAAI ICWSM*. The primary reference for the VADER implementation and the justification for using it over transformer-based alternatives on short headline text.

- Reimers, N., and Gurevych, I. (2019). Sentence-BERT: Sentence embeddings using Siamese BERT-networks. *Proceedings of EMNLP*. The paper behind all-MiniLM-L6-v2, the model used for headline embeddings throughout the bias scoring pipeline.

**Validation and statistics**

- Landis, J. R., and Koch, G. G. (1977). The measurement of observer agreement for categorical data. *Biometrics*, 33(1), 159–174. Used for interpreting the PSI Cohen's kappa of 0.368 as "fair" agreement on the standard scale.

- Stanford HAI. *AI Index Reports 2024 and 2025.* Stanford University. Used in the introduction to contextualize why public perception of AI matters and to ground the project in documented trends.

**Data sources**

- AI News Headlines Dataset (Aug 2024 to Jul 2025). Figshare.  
  https://figshare.com/articles/dataset/Dataset_of_AI-related_News_Headlines_Aug_2024_Jul_2025_/30189736  
  Used as a methodological baseline and for cross-validation. The present corpus extends this dataset from 89 days to 607 days and from 20,581 to 179,372 headlines.

- AllSides Media Bias Ratings. https://www.allsides.com/media-bias/media-bias-ratings  
  Used to join a political lean label onto 118 of 13,019 publishers in the corpus, covering 10.3% of headlines by volume.

---

## C. Software and Libraries

All software used in this project is open-source. Full pinned versions are in `requirements.txt`.

| Library | Version | Purpose |
|---|---|---|
| gnews | 0.3.7 | Google News RSS scraping |
| pandas | 2.2.1 | Data manipulation and CSV I/O |
| numpy | 1.26.4 | Numerical operations |
| nltk | 3.8.1 | VADER sentiment analysis |
| sentence-transformers | 2.7.0 | all-MiniLM-L6-v2 headline embeddings |
| scikit-learn | 1.4.2 | Ridge regression, decision tree, K-means, all metrics |
| scipy | 1.13.0 | Statistical tests (t-test, Pearson r, Spearman rho) |
| matplotlib | 3.8.4 | All figures |
| seaborn | 0.13.2 | Heatmaps and distribution plots |
| networkx | 3.3 | Bias co-occurrence network visualization |
| sqlite3 | stdlib | Database storage (no separate install) |

---

## D. Additional Figures

All 19 figures generated by the analysis are available in the website figure gallery at [https://sv903.github.io/FinalProject](https://sadhanavasanthakumar.github.io/sv903_FinalProject/) under the "All Figures" tab, and in `outputs/figures/` in the GitHub repository. Each figure in the gallery includes a written description explaining what it shows and how to read it.

Figures not included in the main report due to page constraints:

- `07_decision_tree_cm.png` — Decision tree confusion matrix (8 classes, test accuracy 74.4%)
- `08_dt_importance.png` — Decision tree feature importance by Gini score
- `09_cluster_selection.png` — Elbow method and silhouette validation for K-means k selection
- `11_extrapolation.png` — PSI and AAI linear extrapolation to June 2026 with confidence intervals
- `11_forecast.png` — PSI and AAI 3-month forecast panel with fitted trend lines
- `05b_bias_network.png` / `06_bias_network.png` — Bias co-occurrence network graphs with labeled edge weights

---

## E. Project Links

| Resource | Link |
|---|---|
| Interactive Website |[https://sv903.github.io/FinalProject](https://sadhanavasanthakumar.github.io/sv903_FinalProject/) |
| GitHub Repository | [https://github.com/sv903/FinalProject](https://github.com/SadhanaVasanthakumar/sv903_FinalProject/tree/main) |
| README | [https://github.com/sv903/FinalProject/blob/main/README.md](https://github.com/SadhanaVasanthakumar/sv903_FinalProject/blob/main/README.md) |
| Figshare Baseline Dataset | https://figshare.com/articles/dataset/Dataset_of_AI-related_News_Headlines_Aug_2024_Jul_2025_/30189736 |
| Demo Video | [Link to be added] |
