目标: 基于TCGA-LUAD的转录组数据和甲基化数据，构建预后风险评分模型 

技术路线:  


脚本说明：
1> s1_rna_meth_preprocess.ipynb：转录组和甲基化数据的预处理脚本。
2> s2_EC-all_cox_molecular_subtype-DEGs-meth2gene.ipynb：基于102个内皮衰老细胞基因集，筛选单因素cox显著特征(p<0.05)，并基于显著特征进行分子分型，同时针对亚型间差异特征分析。
3> s3_split_DEGs_prognostic_model.ipynb：筛选单因素cox显著的亚型间差异特征(p<0.05)，并利用Lasso-Cox构建预后风险评分模型。
4> S4_analysis_high_low_risk.ipynb: 基于surv_cutpoint()最佳阈值进行高低风险分层，通过整合免疫微环境分析、TIDE评分预测及药物敏感性分析，系统评估模型的预后价值。
5> S5_immunotherapy.ipynb: 利用外部治疗队列IMvigor210和GSE78220来验证模型在预测免疫治疗反应的性能。

结果说明：
result:
1> 

该模型在测试集中KM曲线差异显著(p=0.023<0.05), C-index为0.680，测试集1-year AUC达0.76。
