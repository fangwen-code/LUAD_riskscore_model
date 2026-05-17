目标: 基于TCGA-LUAD的转录组数据和甲基化数据，构建预后风险评分模型。  
亮点：从内皮衰老切入，结合多组学，筛选出的 8 基因风险评分在训练和验证中对 LUAD 预后有稳定的区分能力。

技术路线:   
<img width="510" height="400" alt="image" src="https://github.com/user-attachments/assets/f06f8d82-5383-46da-a969-444934cc782c" />


脚本说明：  
1> s1_rna_meth_preprocess.ipynb：转录组和甲基化数据的预处理脚本。  
2> s2_EC-all_cox_molecular_subtype-DEGs-meth2gene.ipynb：基于102个内皮衰老细胞基因集，筛选单因素cox显著特征(p<0.05)，并基于显著特征进行分子分型，同时针对亚型间差异特征分析。  
3> s3_split_DEGs_prognostic_model.ipynb：筛选单因素cox显著的亚型间差异特征(p<0.05)，并利用Lasso-Cox构建预后风险评分模型。  
4> s4_analysis_high_low_risk.ipynb: 基于surv_cutpoint()最佳阈值进行高低风险分层，通过整合免疫微环境分析、TIDE评分预测及药物敏感性分析，系统评估模型的预后价值。  
5> s5_immunotherapy.ipynb: 利用外部治疗队列IMvigor210和GSE78220来验证模型在预测免疫治疗反应的性能。   

结果说明：    
result/:    
1> 分子分型结果 cluster/：    
a. consensus.pdf: ConsensusClusterPlus共识聚类结果，得到两个亚型;    
b. tSNE_Cluster_K2.pdf: 聚类可视化结果;     
c. KM_K2.pdf: 两个亚型生存曲线差异显著，Log-rank p = 0.00026;    
   
2> 模型性能评估 model_eval/：  
    该模型在测试集中KM曲线差异显著(p=0.023<0.05), C-index为0.680，测试集1-year AUC达0.76。    

3> 预后价值评估 prognostic_eval/：  
    通过ESTIMATE, MCPcounter, TIMER, TIDE评分多个维度来分析免疫微环境，并结合pRRophetic进行药物敏感性分析，发现：高风险组呈现免疫细胞浸润丰富，但 TIDE 评分显著更高(p = 0.00047)。高风险组对传统细胞毒性化疗更敏感，而低风险组对 EGFR-TKI 和 mTOR抑制剂更敏感，这提示高低风险组可能需要不同的治疗策略。  

4> 外部队列验证 cohort_validation/：  
    基于模型在独立队列 IMvigor210（抗 PD-L1）和 GSE78220（抗 PD-1）验证结果，发现：高低风险组并未发现免疫治疗获益。原因是训练数据本身没有免疫治疗标签，而肿瘤内在的预后信号和免疫检查点响应是两套调控机制。  

待优化：  
1> 用惩罚细化的 Cox如elastic net 替代 lasso；  
2> 在现有基础上，添加新特征，如免疫浸润 signature score 作为额外协变量等；  
3> 目前这个模型是一个预后模型，在预测免疫治疗响应上能力有限，后续可以考虑直接拿免疫治疗队列的训练数据来做特征筛选。  
