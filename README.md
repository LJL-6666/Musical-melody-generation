# Musical-melody-generation
## 基于生成对抗网络的音乐旋律生成研究及应用

### 摘要
   音乐是人类历史上最伟大的发明之一。然而音乐因为其专业性需要大量的音乐领域知识和乐器相关技能，平常人不容易开展音乐创作任务，而专业音乐人创作音乐也存在局限。随着深度学习技术的发展，越来越多的作品开始将艺术创作与计算机技术相结合，这已经成为人工智能领域的一个新的研究方向。本文首先梳理了国内外计算机音乐生成的相关研究现状，介绍了本文基于生成对抗网络生成音乐旋律的主要研究内容和方法。然后在相关理论基础部分，普及了音乐的乐理及MIDI格式的知识背景，并详细说明了LSTM网络和生成对抗网络的原理、损失函数等。
随后进入了实验设计及分析部分，依次介绍了实验的环境，说明了使用的音乐数据集及为满足模型需要，采用的数据预处理方法。重点是提出了基于Bi-LSTM-GAN网络的音乐生成模型，并对模型进行了训练、优化和结果分析。实验结果表明，设计的Bi-LSTM-GAN网络的音乐生成模型在训练轮次达到500轮后，模型趋于平衡，并且生成的古筝乐曲经过人工测评，取得了满意的效果。最后在总结与展望部分，先从两个方面对本文做的工作进行了深入的回顾，然后对本论文旋律生成研究的不足和缺陷进行了反思复盘，并提出了基于现阶段未来工作的新的探索计划和研究指导方针，以解决这些措施的不足之处。

### 关键词：
生成对抗网络；Bi-LSTM；音乐生成

1.1  研究背景

音乐是人类历史上最伟大的发明之一。然而音乐因为其专业性需要大量的音乐领域知识和乐器相关技能，平常人不容易开展音乐创作任务，而专业音乐人创作音乐也可能存在局限。能否用机器代替我们创作音乐呢？  
随着深度学习技术的发展，越来越多的作品开始将艺术创作与计算机技术相结合，这已经成为人工智能领域的一个新的研究方向[1]。比如，在图像领域，神经风格转换技术可以将任意一幅图画被转化成梵高大师风格的图画；对抗生成网络通过生成者和鉴别者之间的对抗训练，可以生成非常真实的画面。在文字领域，我们可以使用循环神经网络写出莎士比亚风格的诗歌；微软公司的产品“微软小冰”还可以实现看图写诗。相较于以上二者，尤其是当前火爆的计算机视觉领域，用AI创造音乐更少的被人提及。其中的原因可能由于音乐的主观性特点，结果是否完美是凭人的感觉定义，而且不同的人对同一个曲子的评价可能差别显著。因此，音乐艺术的创作对计算机来说仍然是一个巨大的挑战，有些人认为音乐创作是人工智能所能征服的最后一个领域[2]。  
后疫情时代，音乐领域的线下业务并未完全回暖；各个音乐平台和音乐人等纷纷拓展线上的业务，比如抖音、快手短视频等传媒活动迎合了大众线上娱乐需求。艾媒数据报告给出2018年至2022年我国数字音乐产业市场规模的走势及预测，如图1-1所示。数据表明，我国数字音乐市场规模呈现稳定增长的趋势，2021年我国数字音乐市场规模将扩展到近430亿元，预计于2022年增长至482.7亿元。

<img width="290" alt="image" src="https://user-images.githubusercontent.com/60246446/216769537-ae189a06-4e6b-49d8-92ff-eddf826c5a1b.png">

由此可见，中国在线音乐市场发展前景非常乐观。音乐生成的价值在于它可以帮助音乐创作人创作音乐，提高人们的创造力。如果这一领域能够持续发展，AI必然将对未来的音乐创作提供巨大的帮助。在人工智能的输出大于输入的计算机技术的推动下，我国数字音乐的市场规模将逐步扩大，在线音乐也必将得到欣欣向荣的发展红利。

1.2  研究的目的及意义

1.2.1  研究目的

随着手机、平板电脑等智能终端设备的快速发展，市场关联效应将更加可观。不仅物联网的全面普及让用户对文化娱乐产品提出了多样化的需求，音视频、动漫、游戏、音乐比赛等文化娱乐行业都蕴含着丰富的音乐新鲜元素，需求巨大[3]。这般多的用户基础需求，传统的音乐创作核心模式是无法实现满足短时间高速发展中有音乐特色产业变动的[3]。
音乐是个很大的产业，分高端和低端。人们常接触到的是最具原创性的那部分优秀的音乐人，人工智能必然是无法取代他们的。但是音乐产业里还有很多并不需要那么多原创的工作。比如公共场合的背景音乐，或者一些语音节目的背景音乐，他们并不一定要包含深刻的思想、动人的情绪。它们有时候是功能性的，比如用舒缓的节奏引导人们情绪，不要在过于拥挤的环境里焦躁起来，或者是为了掩盖一些语音中的瑕疵和回音而出现的伴奏曲。很多时候只要有音乐就行，而且并不需要有多么突出。但是在注重版权的地方，哪怕是一首很普通的背景音也要10美元一首。AI可以让这些费用降到几乎为0。为了简化音乐写作的过程，提高创作效率，优秀的音乐作品被AI学习后便可以依据一定的逻辑继续创造新的同风格的优秀作品充盈需求市场，本文以此为目的，研究这个逻辑的细节。

1.2.2  研究意义

在AI快速发展的时代，利用神经网络生成音乐旋律具有许多应用前景[4]。人类的创造是可以找到规律的，这些规律，有很多都可以总结成数学模型，变成算法。算法能够穷尽所有的可能性，带来无限的新东西，但是，“新”并不等于创造。人工智能的科技将越来越明显地激发人类的创造力和音乐家的灵感。据报道，人机合作的创作方法是人类音乐作家创作速度的20多倍。在一定程度上提高音乐家的工作效率，降低艺术创作成本，AI作曲有着人类协作难以匹及的优势。人工智能的一项项挑战人类创造性工作，意义不在输赢、高下，而是让我们获得了一种更高级的工具。这个工具，能让我们看到更多的可能性，提高自身的创造力。
综上所述，AI生成音乐的前景展望和经济效益大家都是有目共睹的，因此探索音乐旋律领域的研究工作具有重要意义。  

1.3  国内外研究现状

1.3.1  国外研究现状

Mohit Dua等人（2020）[5] 通过源分离和弦估计模块分别采用多层GRU和LSTM单元实现RNN，提高了活页乐谱生成的准确性。Broek K（2021）[6]提出了一个深度2D卷积GAN，利用MP3/Vorbis音频压缩技术来生成具有远程一致性的长、高质量的音频样本。该模型使用修正离散余弦变换（MDCT）数据表示，其中包括所有相位信息。利用人耳的听觉掩蔽和心理声学感知极限来扩大真实分布并稳定训练过程。Li G等人（2021）[7] 通过新的预处理方式获得音乐数据，经过LSTM-GAN模型，通过最大平均差异评估验证了模型的有效性。Li Z等人（2021）[8] 引入一种通过GAN生成符号域音乐的方法，对 LSTM 模型进行了比较测试，并根据统计和音乐理论在五个指标上评估随机采样的音乐。结果表明，其模型更能生成连贯、自然和逼真的音乐作品。Bao C等人（2022）[9] 提出了一种无注释构建每个样本都是歌词、旋律和情感标签的三元组的新数据集，先在 GoEmotions 数据集上预训练，训练自动情感识别模型。用它来自动“标记”带有从歌词中识别的情感标签的音乐。然后训练基于编码器-解码器的模型在该数据集上生成情感音乐，将整体方法称为情感歌词和旋律生成器。Huang H I等人（2022）[10] 提出生成器和判别器都是单向 LSTM 网络，用于生成用于视频编辑的镜头转换序列。得出LSTM-GAN 生成的序列质量优于马尔可夫链或 LSTM 生成的质量，同时保证了创造性、继承性和多样性。Zhu Y等人（2022）[11] 提出了一种新颖的对抗性多模态框架 Dance2Music-GAN (D2M-GAN)，将舞蹈视频帧和人体运动作为输入，学习并生成可能伴随相应输入的音乐样本。

1.3.2  国内研究现状

邱燕（2018）[12] 提出了MCT-GAN音乐生成模型，实现了基于多音轨的离散音乐事件的生成方法，生成结果比MuseGAN要好些。苗北辰等人（2019）[13] 从多段音乐序列中提取每个时间步和弦的隐含核心特征，提出将RNN和LSTM的融合的音乐生成模型可以生成多声部旋律。叶文豪（2019）基于BiLSTM和GANs算法的自动合成音乐。该文提出了利用具有时序性的BiLSTM和神经网络组成的混合网络, 使用伪吉布斯采样的方式来生成音乐旋律，最后利用GANs中的判别器来鉴定生成的曲子和真实曲子的数据是否在差异中还有相似，博弈后最终生成旋律，它的生成模型Gi由BiLSTM和Dense构成, 判别模型Di由CNN和Dense构成。白勇（2020）[14]建立了端到端的音乐生成网络Melody_LSTM，然后利用了深度强化学习算法构建了音乐生成模型ACMG。师海波（2021）在多音轨音乐生成模型MuseGAN的基础上，提出了循环特征网络RFGAN模型，通过 Pianoroll 格式的多音轨音乐数据集训练，该模型增强了旋律样本的上下文关联性。武堂颖（2021）以卷积GAN模型Midinet为基本模型，这里分别基于乐理规则、基于和弦特征的DCC_GAN网络和基于整体风格的DCG_GAN网络构建了三个音乐生成模型。汪涛,靳聪,李小兵,帖云,齐林（2021）[15]提出了新型多乐器多音轨旋律生成模型Transformer-GAN，以音乐理论规则为指导，生成具有高度音乐性的音乐作品。

1.3.3  研究现状综述

综上所述，对于计算机音乐创作而言，国外在该领域的发展更早于国内[12]。对于计算机生成音乐的相关学术研究文献，整理音乐生成方法大致分为三类，分别为：
（1）基于概率模型的方法。该方法采用了有一定局限性的隐马尔科夫模型，由于马尔可夫的假设状态的变化模式是线性成比例的，因此很难了解长期依赖性。同时，隐式马尔科夫模型仅适用于数据较少的情况。
（2）基于随机选择和组合的方法。该方法不仅需要通过大量实验检验，还需要进行多次观察和优化，乐曲的创作具有较大的主观性，稳定性也比较差。
（3）基于神经网络的方法。该方法的主要问题是音乐的相关特征复杂，训练方法又是单一的，不能确保生成旋律的质量如何。然而，基于神经网络的音乐旋律生成方法，不需要大量的人力和非常专业权威的音乐理论储备。只要对模型进行训练，旋律的生成速度非常快，理论上说可以批量生成大量新的音乐旋律[14]。
本文将基于生成对抗网络，设计训练深度学习模型，与叶文豪的模型BiLSTM-GANs不同在于目的和数据训练类型以及模型结构的组成，自动生成更多的适合广大中国人民喜爱的音乐，丰富大家的精神生活，为数字音乐的发展助力。

![image](https://user-images.githubusercontent.com/60246446/216770587-aa5b85d6-9a5a-42b0-9159-c0c6083f3e40.png)

3.1  实验设置

由于GAN和LSTM都是一种无监督的深度学习模型，所以对音乐生成实验的软件和硬件的需求都比较高。比如，需要给实验搭建一个专属的虚拟环境来配套操作，而且实现旋律的生成和应用需要使用多种工具库，每个工具库的版本得相互兼容才行。
本文使用了一些关于生成旋律python编译语言下的相关工具库，具体版本如表3-1。
表3-1  Python及相关库表
工具库	版本号
python	2.3.0
music21	7.1.0
numpy	1.18.5
matplotlib	3.3.4
keras	2.4.3
   
本实验计划实现将 MIDI 文件中的音符和和弦视为离散的序列数据，通过训练模型能够创作出与实际音乐相混淆的全新的 MIDI 旋律文件，换句话是说，依据一定的逻辑继续创造新的同风格的优秀作品，最后分别让古筝老师和学院对其进行打分。

3.2  音乐数据集介绍
古筝作为中国的传统弹拨各类乐器，音域宽广优美，中音区广阔、视觉表现力强，深受广大文人雅士的深受。本文收集了30首古筝演奏的知名音乐。音乐的文件名均为MIDI格式，具体歌曲名称如下表3-2。
由于MIDI格式能传输音符、控制参数和其他指令，不能传输声音信号，因此它需要告诉计算机设备相关数据指标，例如播放什么音符、音量具体情况等。使用pretty_midi，查看“将军令”这支古筝曲目的notes.部分截图如图3-1。
其中，start和end分别表示这个音符的起止时间，单位是秒。在同一个时刻可以有多个音符同时演奏，所以多个音符的起止时间可以重叠。pitch代表这个音符的音调，velocity代表这个音符的强度。图3-1表示了pitch如何与某一个音符对应。通常来说，这里有128个音符被MIDI定义，中央C的编号为60，5个八度的键盘编号可能就是36到96。  

表3-2  歌曲表

序号	歌曲名称	序号	歌曲名称
1	将军令.mid	16	汉宫秋月.mid
2	香山射鼓.mid	17	河南八板.mid
3	三十三板.mid	18	浣花语.mid
4	青山隐.mid	19	海之波澜.mid
5	丰收锣鼓.mid	20	渔舟唱晚.mid
6	井冈山上太阳红.mid	21	灯月交辉.mid
7	卧虎藏龙.mid	22	畲山茶歌.mid
8	四合如意.mid	23	紫竹调.mid
9	小雨.mid	24	绣金匾.mid
10	尘香.mid	25	自然风自然海.mid
11	山的遐想.mid	26	自由探戈.mid
12	工人赞.mid	27	草原英雄小姐妹.mid
13	战台风.mid	28	莲花谣.mid
14	春色如许.mid	29	蕉窗夜雨.mid
15	柳青娘.mid	30	西江月.mid

<img width="322" alt="image" src="https://user-images.githubusercontent.com/60246446/216769729-1f519aa2-5f54-499d-98ad-b693a464a08d.png">

图3-1 《将军令》的Notes

类似Note(start=0.000000, end=0.054348, pitch=78, velocity=18)的数据是无法直接传递给神经网络的，因此需要一步一步的对原始数据进行处理，使其能够作为神经网络的输入。

3.3  音乐数据预处理

3.3.1  生成字符串列表

由于一个.mid文件中通常有不止一个音轨，这里为了方便建立模型，本文只选取了最主要用的音轨，即用钢琴弹奏的音轨（位于第1个音轨上）。为方便处理音乐数据，后续采用更为高级的music21库。

<img width="283" alt="image" src="https://user-images.githubusercontent.com/60246446/216769740-195aaa4d-2d04-418e-b28f-642d07761bc4.png">

图3-2  音轨分割后的示意图

从上图3-2可以看出“将军令”这首古筝曲在钢琴弹奏的音轨上的音符和和弦信息。为了方便后续处理，这里将单音符的音调提取出来，转化为字符串。同时将和弦也提取出来，并返回用整数表示的每个音调的正常顺序。“将军令”转换的示意图如下所示。

<img width="214" alt="image" src="https://user-images.githubusercontent.com/60246446/216769746-24bedfd0-1ac9-46dd-9c72-36ca64136f9c.png">

图3-3  乐曲转为字符串示意图

从图3-3可以看出，“将军令”中的第一个和弦转换为了‘5.6’，第三个和弦转换为了‘9.11.4’，一个单音符转换为了‘E6’。剩余其他29首乐曲均需进行上述转换，最后合并存储在一个列表中。该列表的长度为84804。

3.3.2  创建音符字典

为方便后续处理，将字符串列表去重后，建立了Python字典。这样，每段音乐都可以用字符串对应的数字表示，如图3-4。 

<img width="194" alt="image" src="https://user-images.githubusercontent.com/60246446/216769758-9873d0bf-95c4-4afe-ab94-f065f0ecaeef.png">

图3-4  音符字典截图

3.3.3  创建音符序列

作为形成深度学习的序列生成模型来说，训练样本的输入为一段连续时间内的值，阶段目标输出为下一时间步的值。本文争取技术手段当前的一段时间的输入值，来预测下一个时间步的输出值。对于音乐生成来说，本文的输入为一段时间的音符，目标输出为下一个时间步的音符，如图3-5。

<img width="301" alt="image" src="https://user-images.githubusercontent.com/60246446/216769765-cedc5043-4b68-46b6-84d5-11d3b7e22ec6.png">

图3-5  输入输出示意图

这里比如将音符序列长度设置为100。模型的输入为连续100个的音调，目标输出为第101个音调。30首古筝乐曲的音符列表长度为84804，以100个截断，可以构成84704个输入序列，每个输入序列也对应一个输出音调。这里展示第一个序列如图3-6，即前100个音调转为数字后的输入和其对应的输出。

![image](https://user-images.githubusercontent.com/60246446/216770313-c2d76ff0-991b-48f5-bfc9-0e375be71fed.png)

图3-6  第一个序列图

3.4  基于Bi-LSTM-GAN的网络模型设计

GAN算法首先在图像领域内应用生成了新的图像，GAN能够根据给定的图像，给出风格类似的新图像。在图像生成应用中，GAN模型实质上是高斯分布的固定概率，转化为包含在训练数据中的概率分布。据流形几何分析GAN的原理，在高维空间内，一个图像可以是高维空间内的一个点，同一类图像在高维空间中流形距离较近。对于给定的一个分布的数据点，通过GAN算法的迭代过程可以将给定分布的数据集合映射到训练集图像的概率测度上。同理可以类比，将一段音乐序列作为高维空间中的点，相近的音乐在高维空间形成一簇距离较近的点，给定一个基准，通过GAN的方法，理论上也可以将给定分布的序列映射到训练集音乐的概率测度上。当给定的序列通过生成网络映射在高维空间上与训练集的音乐序列点距离较近，但是又不是完全重合的，这个时候如果给定一个数据点通过生成网络映射的音乐具有了训练集音乐的风格，但又不是原来的音乐，因此生成了新的音乐，这就是模型采用GAN的优势。
根据GAN的生成新数据功能，本研究需要提出一个可以自动生成新的音乐旋律的智能模型，对GAN算法提出改进，以适用于序列数据的生成。由于音乐乐曲是由音符按一定顺序排列构成的，而音符序列可以看成时间序列，LSTM模型适合处理序列数据，进一步，如果一个序列下一个时间步的值只和之前的信息有关，仅需使用单相LSTM网络即可，但更实际的情况往往是序列某一个时间点的值，它即与之前的信息有关，又和之后的信息有关。音乐也是一样，尤其是中国古典音乐，都会有一些重复的旋律，因此如果我们在预测某一个时间点的音符时使用了前后的信息，可以使预测的精度获得更大的提升。因此，在本次实验模型的判别器模型的第一层我们使用双向LSTM网络。 经过了双向循环网络得到特征信息后，还需使用全连接层进一步获取深层次的特征，最后经过激活函数sigmoid获得生成的音乐是否能“以假乱真”的概率。
基于以上原因，我们设计了一个基于Bi-LSTM-GAN的音乐生成模型，详细网络结构如下：

（1）生成器网络结构

<img width="261" alt="image" src="https://user-images.githubusercontent.com/60246446/216769788-70306253-7d37-44c1-8179-bbb66551e387.png">

图3-7  生成器网络结构示意图

该3-7网络结构的生成器采取4层的神经网络结构，输入随机产出的序列后，先经过有256个神经元的全连接层，此层采用的激活函数是LeakyRelu，并做批标准化；然后通过与512个神经元全连接的层，该层仍然使用LeakyRelu作为批量激活，并做批标准化，然后通过与1024个神经元全连接的层，该层仍然使用LeakyRelu作为批量激活，并做批标准化；最后的输出层有100个神经元，激活函数采用tanh,并在输出将数组塑形成三维数组。该部分模型摘要展示如表3-3：

表3-3  生成器模型摘要表

Layer (type)	Output Shape	Param #
Dense_1(Dense)	(None, 256)	256256
leaky_re_lu(LeakyReLU)	(None, 256)	0
batch_normalization (BatchNorm)	(None, 256)	1024
dense_2(Dense)	(None, 512)	131584
leaky_re_lu_2(LeakyReLU)	(None, 512)	0
batch_normalization_1(BatchNorm)	(None, 512)	2048
dense_3(Dense) 	(None, 1024)	525312
leaky_re_lu_3(LeakyReLU)	(None, 1024)	0
batch_normalization_2(BatchNorm)	(None, 1024) 	4096
dense_4(Dense)	(None, 100)	102500
reshape (Reshape)	(None, 100, 1)	0
Total params: 1,022,820
Trainable params: 1,019,236
Non-trainable params: 3,584
从上述模型摘要中也可以发现，模型中参数数量较多，后续将采用多轮训练的方式，以获得效果较好的参数组合。

（2）判别器网络结构

<img width="279" alt="image" src="https://user-images.githubusercontent.com/60246446/216769792-4caecec0-4b98-4ccb-b50f-1a06569a03ff.png">

图3-8  判别器网络结构示意图

该网络的判别器采取5层的神经网络结构，输入真实的乐曲序列（要求输入为100*512的数组），先经过有双向LSTM层，然后它通过由512个神经元组成的完全连接层，该层使用LeakyRelu作为激活函数，然后通过由256个神经元和64个神经元组成的全连接层[23]，两者都使用LeakyRelu作为激活函数；最后的输出层输出生成的古筝旋律能以假乱真的概率，激活函数采用sigmoid。该部分模型的摘要展示如下：

表3-4  判别器模型摘要表

Layer (type)	Output Shape	Param #
lstm (LSTM)	(None, 100, 512)	1052672
Bidirectional (Bidirectional	(None, 1024)	4198400
Dense_5(Dense)	(None, 512)	524800
leaky_re_lu_4(LeakyReLU)	(None, 512)	0
dense_6(Dense)	(None, 256)	131328
leaky_re_lu_5(LeakyReLU)	(None, 256)	0
dense_6(Dense)	(None, 1)	257
Total params: 5,907,457
Trainable params: 5,907,457
Non-trainable params: 0
这部分模型的参数也较多，有5907457个参数，并且都是需要经过训练得到的参数，依然将采用多轮训练的方式，以获得效果较好的参数组合。

3.5  模型训练

在模型训练中，Bi-LSTM-GAN模型采用交叉熵作为损失函数，此时的优化器选择为adam[24]，在判别模型中，随机生成一段在0到1间的正态分布的batch_size行，100维的数据作为噪音。每次生成器都会生成含有100个音符的序列，判别器会对生成的序列进行判别，当生成器和判别器“博弈”结束后，我们用训练后的生成器，生成一个由500个音符序列，并将其组合在一起。我们再将这个序列采用数据预处理的逆步骤，就会得到一首MIDI格式的古筝曲目。模型训练时的初始参数设置如表3-5。

表3-5  初始参数
参数	参数名称	参数值
momentum	批标准化参数	0.8
alpha	LeakyReLU函数参数	0.2
learning_rate	学习率	0.0002
batch_size	批量	10

本次实验一共训练了100轮，判别器D和生成器G的损失值Loss的变化如图3-9所示。

<img width="292" alt="image" src="https://user-images.githubusercontent.com/60246446/216769644-5d3252a4-3bf2-431b-947d-5b2877ed59a1.png">

图3-9  判别器和生成器损失变化图

从图3-9可以看出，生成器损失值变化震荡明显，但整体来所震荡情况随着训练轮次的增加，震荡幅度再减小。判别器情况较为稳定，但有局部的突起。

<img width="293" alt="image" src="https://user-images.githubusercontent.com/60246446/216769657-2c0dadec-3947-46ea-810a-a80909575047.png">

如图3-10中该模型判别器的准确率在100轮次的训练过程中是震荡变化的。从该图中可以看出，在训练到80轮后，判别器的判别准确率开始震荡下移。

3.6  模型优化
为了达到模型最好的效果，使得网络产生的音乐能达到“以假乱真”的效果，在模型结构不变的情况下，本节分别取lr、momentu和alpha不同的值对模型进行训练，这里展示取不同参数值时判别器准确率变化情况。

<img width="278" alt="image" src="https://user-images.githubusercontent.com/60246446/216769813-62123508-65df-4f9e-a285-d5a1e3a52a33.png">

图3-11  learning_rate不同时判别器精确度变化图

<img width="280" alt="image" src="https://user-images.githubusercontent.com/60246446/216769818-605cd714-94eb-4238-8015-d7eddb964636.png">

图3-12  monentum不同时判别器精确度变化图

<img width="272" alt="image" src="https://user-images.githubusercontent.com/60246446/216769827-2d592459-dfff-421e-b888-77a2621cc41a.png">

图3-13  alpha不同时判别器精确度变化图

从上面图3-11、3-12和3-13的情况来看，模型在取不同参数值时，变化趋势基本一致。故这里保持原始参数值不变，通过增加训练轮次和批量达到让模型收敛的效果。当训练轮次增加到500，批量增加到20时，模型达到收敛的效果。详见下图。

<img width="274" alt="image" src="https://user-images.githubusercontent.com/60246446/216769837-4654bfbf-38c5-4e1a-aaa0-487ca4a70f36.png">

图3-14  损失变化图

<img width="278" alt="image" src="https://user-images.githubusercontent.com/60246446/216769846-4ee73f2d-c432-418f-b8c6-bfefd0ea971c.png">

图3-15  判别器准确率曲线

从图3-14中可以看出，随着训练次数增加，生成器在训练200次内的损失从6开始迅速下降，鉴别器的损失则从0.3开始缓慢上升，最终在训练300次后二者皆稳定于损失为1的附近，由此得出训练效果不错。再看图3-15判别器准确率曲线，500次的训练中准确率从100降到30到40之间，判别器变差，开始将假的数据判别成真的, 判断的准确率下降说明生成器训练的足够好。

3.7  模型结果分析

本节选取了生成的1段古筝音乐（MIDI）格式，为了便于查看，将其转换为五线谱，显示如下：
![image](https://user-images.githubusercontent.com/60246446/216769875-10f4801c-20e6-49f2-8c1e-7ac0cb485f26.png)

图3-16  生成的古筝音乐的五线谱

为进一步评估生成的古筝音乐序列的专业性，我们找了50名古筝教师和50古筝学员，让他们听生成的古筝乐曲，并给与评分（满分10分），评分标准如下：

表3-6  评分标准表
分数	详情
1-3分	太糟糕了，这就不是一段古筝曲子
4-6分	这是一段不悦耳的古筝曲子
7-8分	这是一段还不错的古筝曲子
9-10分	这是一段非常悦耳的古筝曲子

<img width="238" alt="image" src="https://user-images.githubusercontent.com/60246446/216769881-25589fe6-b302-484b-91e5-b12d607a030e.png">

图3-18  古筝教师评分结果直方图

经过100位测评者的试听评分，统计得到的评分结果如下。由图3-18展示了50名古筝老师的评分情况，呈正态分布，众数集中在6-7分之间。古筝学员的评分结果如图3-19，整体也呈正态分布，但众数集中在7-8分之间，由此可见，该模型生成的古筝音乐还是不错的。

<img width="242" alt="image" src="https://user-images.githubusercontent.com/60246446/216769885-7e681aaf-3269-448f-a391-93bdc1cba5b6.png">

图3-19  古筝学员评分结果直方图

4  总结与展望

4.1  总结

疫情下的生活虽然限制了人们的出行，但对生活质量的追求矢志不渝，闲暇时间和休息时间的增加，自然而然人们的精神需求也就越来越大，BGM的流行，带给人们在精神世界美的感受毋庸置疑。其实，在人工智能领域，生成真实且具有设计美感的音乐作品被认为是一种挑战。本文主要围绕基于生成对抗网络的音乐生成进行了研究和应用，为此展开以下工作：
本文首先介绍了音乐生成的研究背景、目的和重要性，并通过对国内外研究现状的综述，提出了本文的研究内容和实践方法。对理论基础包括乐理、LSTM网络、GAN网络和神经网络常用损失函数的学习后，设计了音乐生成的Bi-LSTM-GAN网络模型。实验部分本文将古筝音乐数据数字化以后，利用双向LSTM和GAN模型成功地生成了古筝的mid格式音乐。虽然通过实验实现了音乐旋律的生成，但同时也存在以下问题：  
（1）在本文实验所生成的旋律片段中，仔细观察好多生成的旋律片段和真实的原音乐数据相比，带有一些分散的音符，这样致使生成的古筝音乐旋律和原始古筝名作相比还不足以让人魂牵梦绕。  
（2）本文的古筝音乐数据集仅对第一个音轨的音乐进行生成，然而现实生活中，一个美的音乐必然还存在着别的音轨与其相辅相成，并且不同的音乐旋律包含的音轨种类和数量也都是不同的，这里的不确定性给该课题的研究实践带来一定的困难。
本文关于音乐旋律生成的研究成果能够降低艺术创作成本，为大众在娱乐时间进行个性化音乐选择提供技术支持，为推进我国艺术创造事业以及音乐产业的发展提供一定理论指导。

4.2  展望

近年来，权威学者对计算机音乐生成方面的研究越来越多，鉴于本文研究实践存在的不足和问题，仍有许多方向值得进一步探索，分别有以下几个方面：  
对于实验中音乐数据集方面，本文生成的古筝音乐只包含了第一个主音轨，然而，常被放在耳边的音乐创作的旋律则会包含更多不同的音轨，因此，在未来的工作中，旋律生成领域的实践研究者可以试验具有多样性的音乐数据集，来生成包含4个音轨以上的音乐旋律，使计算机生成的音乐旋律不光可以“以假乱真”，还能具有更广阔的可能性。除此之外，音乐是一种具有复杂结构特征的艺术审美，它的整体结构是有特定形式的，每一首曲目的结构组成当然也尽不相同。本文是没有考虑某段曲目旋律的结构组成因素的，所以在接下来的工作中，同领域工作者可以研究如何将曲目旋律的结构特征添加到创新的网络模型中，以生成完整的音乐作品。 

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
