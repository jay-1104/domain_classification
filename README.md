# Domain Classification

A complete end-to-end pipeline for detecting malicious domains with high recall and very low false-positive rate. This project covers:

1. **Feature Engineering**  
   - Lexical URL features (lengths, token statistics)  
   - Domain token statistics  
   - Host-based reputation (“rogue index”) from WHOIS / DNS (name servers, registrars, ASNs)  
   - DNS counts (NS, MX), ASN country codes, TLD flags  
   - Textual features (char- and word-level TF–IDF, HashingVectorizer + SVD)

2. **Preprocessing**  
   - 97.5th-percentile clipping + min–max scaling on continuous features  
   - Rogue-index features left in [0,1]  
   - Binary flags cast to 0/1  
   - Vectorization of all text columns with safe SVD reduction  

3. **Feature Selection**  
   - **LASSO** screening (L1-penalized logistic regression) → 4 strongest predictors  
   - **Genetic Algorithm** (Pareto GA with SVM) to pick an additional 29 features → 33 total  

4. **Classification & False-Positive Reduction**  
   - Trained five models: Random Forest, LightGBM, SVM, Logistic Regression, K-NN, Naïve Bayes  
   - Class-weight tuning, isotonic probability calibration, and precision-first thresholding (Precision ≥ 0.90)  
   - Post-filter rule: drop flagged positives with no NS records and very young site age  
