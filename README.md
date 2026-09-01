# A-Survey-on-Machine-Learning-and-Deep-Learning-Approaches-for-DDoS-Attack-Detection
● # A Survey on Machine Learning and Deep Learning Approaches for DDoS Attack Detection
  
  **Methods, Datasets, and Emerging Applications**

  Nihal Baba Mohammad · Manognya Jupally
  Department of Computer Science, Cleveland State University
  Course project · 2025–2026

 
  ---                                                                                                                                                                                                     

  ## Overview

  Distributed Denial of Service attacks remain one of the most damaging threats to
  networked systems — attack volumes grew over 50% in 2024, with peaks above
  1.6 Tbps. Signature-based intrusion detection cannot keep up with novel or
  obfuscated variants, which has pushed the field toward machine learning.

  This survey reviews ML and DL approaches to DDoS detection published between
  2024 and 2026, covering detection methods, benchmark datasets, feature
  engineering strategies, and the practical constraints of real-world deployment
  across cloud, SDN, IoT, and autonomous-vehicle environments.

  ---
                                                                                                                                                                                                          
  ## Key finding

  **Reported accuracy depends heavily on how the model is evaluated.** The same
  class of detectors produces very different numbers depending on the setup:
  
  | Evaluation setting | Reported accuracy |                                                                                                                                                              
  |---|---|
  | Binary (benign vs. attack) | **99%+** |
  | Multiclass (9 attack categories) | **91.26%** |
  | Under adversarial attack (PGD) | **71%** |

  Most published work reports only the first number. Since real deployments face
  multiple simultaneous attack types and adaptive adversaries, benchmark accuracy
  substantially overstates real-world detector performance.

  A secondary finding: **deep learning is not universally better.** In a
  data-constrained smart-home setting, KNN reached 97.13% while an ANN managed
  only 81.7% — model complexity should match deployment constraints, not default
  to the largest available architecture.

  ---
                                                                                                                                                                                                          
  ## What the survey covers

  **Attack taxonomy** — volumetric, protocol/state-exhaustion, application-layer,
  and low-rate attacks, plus the additional complexity introduced by multi-tenant
  cloud infrastructure.

  **Detection generations** — signature-based → statistical/anomaly-based
  (CUSUM, EWMA, entropy) → learned models.
  
  **Datasets** — CIC-DDoS2019, CIC-IDS2017, NSL-KDD, evaluated for class                                                                                                                                  
  imbalance, temporal staleness, and the gap between testbed and production
  traffic.

  **Classical ML** — KNN, Naive Bayes, SVM, Decision Trees.

  **Ensembles** — Random Forest, XGBoost, CatBoost, and hierarchical stacking.
  These consistently outperform single models across the reviewed studies.

  **Deep learning** — CNNs, RNN/LSTM, DNNs, plus federated learning for                                                                                                                                   
  privacy-preserving collaborative training and active learning for label
  efficiency.
                                                                                                                                                                                                          
  **Feature engineering** — SHAP-based interpretable selection and
  domain-specific features derived from SDN OpenFlow statistics.

  ---
  
  ## Reviewed studies                                                                                                                                                                                     

  | Study | Method | Domain | Accuracy | Task |
  |---|---|---|---|---|
  | Samy et al. (2026) | Optimized CatBoost + SHAP | Cloud VMs | 99.2% | Binary |
  | Angulo et al. (2026) | 3-layer stacking ensemble | General | 91.26% | 9-class |
  | Djenna et al. (2026) | DNN + federated learning | Autonomous vehicles | 99.98% | 3-type |
  | Raja et al. | KNN vs. ANN | Smart home / IoT | 97.13% | Binary |
  | Owusu et al. (2024) | Survey: CPD + ML + sampling | General | — | Survey |

  ---

  ## Open challenges identified                                                                                                                                                                           

  - **Adversarial robustness** — accuracy falls from 99.2% to 71% under PGD;                                                                                            
    almost no work evaluates this                                                                                                                                                                         
  - **Multiclass detection** — most research stops at binary, but mitigation
    strategy depends on attack type
  - **Real-time constraints** — complex ensembles may not meet line-rate
    throughput requirements
  - **Dataset representativeness** — lab-generated traffic does not reflect
    production networks, and benchmarks go stale quickly
  - **Encrypted traffic** — payload inspection is no longer viable; detection
    must rely on flow metadata
  - **Cross-domain generalization** — cloud-trained models transfer poorly to
    vehicular or IoT networks
  
  ---                                                                                                                                                                                                     

  ## Related work in this repo

  The multi-class evaluation argument in this survey motivated a companion
  implementation: [**ddos-detection**](https://github.com/NihalMD7/DDOS.git)
  — feed-forward and 1D-CNN classifiers over 13 CICDDoS2019 classes, reaching
  88.3% accuracy / 0.91 weighted F1 on 430k+ flows.

  ---
  
  ## Citation                                                                                                                                                                                             

  ```bibtex
  @misc{mohammad2026ddossurvey,
    title  = {A Survey on Machine Learning and Deep Learning Approaches for
              DDoS Attack Detection: Methods, Datasets, and Emerging Applications},
    author = {Mohammad, Nihal Baba and Jupally, Manognya},
    year   = {2026},
    note   = {Course project, Cleveland State University}                                                                                                                                                 
  }
