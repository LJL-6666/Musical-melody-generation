# Musical-melody-generation
## 基于生成对抗网络的音乐旋律生成研究及应用

### 摘要
   音乐是人类历史上最伟大的发明之一。然而音乐因为其专业性需要大量的音乐领域知识和乐器相关技能，平常人不容易开展音乐创作任务，而专业音乐人创作音乐也存在局限。随着深度学习技术的发展，越来越多的作品开始将艺术创作与计算机技术相结合，这已经成为人工智能领域的一个新的研究方向。本文首先梳理了国内外计算机音乐生成的相关研究现状，介绍了本文基于生成对抗网络生成音乐旋律的主要研究内容和方法。然后在相关理论基础部分，普及了音乐的乐理及MIDI格式的知识背景，并详细说明了LSTM网络和生成对抗网络的原理、损失函数等。
随后进入了实验设计及分析部分，依次介绍了实验的环境，说明了使用的音乐数据集及为满足模型需要，采用的数据预处理方法。重点是提出了基于Bi-LSTM-GAN网络的音乐生成模型，并对模型进行了训练、优化和结果分析。实验结果表明，设计的Bi-LSTM-GAN网络的音乐生成模型在训练轮次达到500轮后，模型趋于平衡，并且生成的古筝乐曲经过人工测评，取得了满意的效果。最后在总结与展望部分，先从两个方面对本文做的工作进行了深入的回顾，然后对本论文旋律生成研究的不足和缺陷进行了反思复盘，并提出了基于现阶段未来工作的新的探索计划和研究指导方针，以解决这些措施的不足之处。

### 关键词：
生成对抗网络；Bi-LSTM；音乐生成



### 参考文献
[1]朱洪渊.基于深度学习的自动作曲编曲研究[D].中国科学技术大学,2019. 

[2]叶文豪.基于BiLSTM和GANs算法的自动作曲[D].吉林大学,2019.

[3]师海波.基于生成对抗网络的上下文相关的多音轨音乐生成[D].西北民族大学,2021.

[4]武堂颖. 基于和弦约束的GAN网络的双轨音乐生成[D].西北民族大学,2021.

[5]Dua M , Yadav R , Mamgai D , et al. An Improved RNN-LSTM based Novel Approach for Sheet Music Generation [J]. Procedia Computer Science, 2020, 171:465-474.

[6]Broek K. MP3net: coherent, minute-long music generation from raw audio with a simple convolutional GAN[J]. 2021.

[7]Li G, Ding S, Li Y. Novel LSTM-GAN Based Music Generation[C].2021 13th International Conference on Wireless Communications and Signal Processing (WCSP). IEEE, 2021: 1-6.

[8] Li Z, Guo Y, Liu Y, et al. A Symbolic-domain Music Generation Method Based on Leak-GAN[C].2021 3rd International Academic Exchange Conference on Science and Technology Innovation (IAECST). IEEE, 2021: 549-552.

[9]Bao C, Sun Q. Generating Music with Emotions[J]. IEEE Transactions on Multimedia, 2022.

[10]Huang H I, Shih C S, Yang Z L. Automated video editing based on learned styles using LSTM-GAN[C].Proceedings of the 37th ACM/SIGAPP Symposium on Applied Computing. 2022: 73-80.

[11]Zhu Y, Olszewski K, Wu Y, et al. Quantized GAN for Complex Music Generation from Dance Videos[J]. 2022.

[12] 邱燕.基于生成对抗网络的音乐生成研究[D].电子科技大学,2019.

[13]苗北辰,郭为安,汪镭.隐式特征和循环神经网络的多声部音乐生成系统[J].智能系统学报,2019,14(01):158-164.

[14]白勇.基于深度强化学习的音乐生成研究与实现[D].郑州大学,2020.

[15]汪涛,靳聪,李小兵,帖云,齐林.基于Transformer的多轨音乐生成对抗网络[J].计算机应用,2021,41(12):3585-3589.

[16]陈刚.基于内容的相关反馈式音乐检索方法研究[D].华中科技大学,2010.

[17]徐国现.基于参数修剪和共享的深度神经网络模型压缩方法研究[D].东南大学,2019. 

[18]申豪杰.基于知识图谱的电影知识问答系统研究与实现[D].重庆师范大学,2019.

[19]宋梦雪.基于自然语言处理的教育领域知识图谱的构建[D].青岛理工大学,2020.

[20]田晗.基于深度学习的雨天交通视频的显著性区域检测[D].电子科技大学,2021.

[21]刘文杰,陈耀,宋晓宁,张万强.基于时域卷积网络精细化光伏发电功率预测[J].供用电,2020.

[22]李质轩.融合上下文信息的汉语分词方法研究[D].北京交通大学,2018.

[23]纪志鹏.基于视频的人脸年龄估计算法研究[D].北京交通大学,2019.

[24]李子耀.基于深度学习的换向器产品质量检测[D].广东工业大学,2021.
