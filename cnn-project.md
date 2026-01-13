---
layout: default
---


# 臺北市私立薇閣高級中學114學年度第1學期工程專題
#### 二己44 蔡旻希
---
# 一、主題：
## $\color{red}{\text{實際運行分析Teachable Machine與SqueezeNet圖像辨識差異}}$
## Teachable Machine介紹：
 Teachable Machine 是一個由Google於2007年推出的深度學習影像分類訓練工具，能夠讓使用者進行簡單的步驟即可訓練出自己的一套模型，能夠辨識影像、聲音與姿勢。其簡化操作與支援多分類轉換適合新手輕鬆學習上手深度學習的使用。
## SqueezeNet介紹：
 2016年，DeepScale、加州大學柏克萊分校和史丹佛大學的研究人員提出了一個緊湊高效的捲積神經網路（CNN）－SqueezeNet。SqueezeNet能夠在保持高準確性的同時，減少模型的複雜度和運算資源的需求。 由於SqueezeNet的緊湊設計和高效運算，它在計算資源受限的場景中具有很大的優勢。
# 二、收集資料：
## 1.資料篩選：
  資料總共有三組，分別為cat、pineapple、basketball的各組資料，我從google上下載數十張照片，並將資料各個篩選，最後留下檔案類型為
  $\color{red}{\text{jpg/jpeg/png的原始資料三組各六十四張}}$
。在cat此類圖片的選擇上，除了貓的正面照外，我也選了幾張能涵蓋到貓的身體的圖片，或是它們的側面圖。bas類的話除了選擇最常見的橘色籃球之外，也選擇了其他顏色的籃球，讓後續訓練出的模型不會只局限於橘色球的選擇。pin類則將鳳梨的切片圖片也選擇，讓模型能夠判斷除了鳳梨的外觀更包還其內的顏色或切片後的型態，這樣在選擇包含有切片的圖做為測試時能夠更精準的判斷。

| <img width="597" height="230" alt="image" src="https://github.com/user-attachments/assets/3a5fc35b-cb04-44db-a25d-17710db43de3" /> |
| :--- |
| ▲資料分成 bas、cat、與 pin 三類 |

## 2.資料預處理：
我使用xnview來進行資料的批次轉換，將所有資料都轉換成全彩圖，並將檔案類型轉換成jpeg。
## 3.格式轉換：
將資料都轉換之後，再後續將資料調整尺寸寬度跟高度的上限為
$\color{red}{\text{227x227}}$
，並維持原圖片比例，放入三組不同的訓練用資料夾中，將圖片以組名加上編號001到064重新編排，以便進行後續的訓練。

| <img width="398" height="354" alt="image" src="https://github.com/user-attachments/assets/85519290-3a95-4e23-aa63-4e7300bed190" /> |
| :--- |
| ▲原始資料的格式轉換 |

# 三、建立模型挖掘資料：
##  SqueezeNet訓練過程：
將資料以如下的設定匯入，
$\color{red}{\text{訓練資料百分率設定為70%，30%用來驗證資料}}$。

| <img width="594" height="314" alt="image" src="https://github.com/user-attachments/assets/7e17b709-fac6-491a-9a63-97f6312b50bd" /> |
| :--- |
| ▲匯入資料的設定(SqueezeNet) |

## 2.Matlab內建模型的調整
我選擇用matlab上的SqueezeNet模型進行調整：
1. 改變輸入層，換成 $\color{red}{\text{圖片的輸入(Imageinputlayer)}}$ <br>

| <img width="428" height="318" alt="image" src="https://github.com/user-attachments/assets/3440d78c-8f53-481f-83af-88a32b56bff7" /> |
| :--- |
| ▲輸入層的修改(SqueezeNet) |

2.改變
$\color{red}{\text{最後一個卷積層}}$
，將參數調成此次訓練的資料對應的大小、數值
| <img width="299" height="428" alt="image" src="https://github.com/user-attachments/assets/2220726d-0cf2-4ff3-9c19-850be826764b" /> |
| :--- |
| ▲卷積層的設定(SqueezeNet) |

3.將輸出層的類別數改成自動判定
| <img width="559" height="377" alt="image" src="https://github.com/user-attachments/assets/f73dc1d6-05bf-4e5e-9744-8c66137f1cd2" /> |
| :--- |
| ▲輸出層的修改(SqueezeNet) |

4.分析錯誤，確認修改後的模型有無缺漏
| <img width="401" height="138" alt="image" src="https://github.com/user-attachments/assets/2d306465-bf79-4fdb-be7f-4728d7fe1473" /> |
| :--- |
| ▲分析修改過後的模型是否有錯誤(SqueezeNet) |

## Teachable Machine訓練過程：
### 1.資料匯入
跟SqueezeNet訓練過程一樣，將一開始處理好的三組類別的圖片匯入。
| <img width="331" height="327" alt="image" src="https://github.com/user-attachments/assets/30f4cafd-9e97-4350-bdd3-0ad7d0130ffc" /> |
| :--- |
| ▲資料匯入與組數設定(Teachable Machine) |


訓練設定則與前面修改過的SqueezeNet一樣，Epochs設為16次，Batch size設為64，訓練率一樣設定成0.0001來進行訓練。


| <img width="251" height="386" alt="image" src="https://github.com/user-attachments/assets/dc12e597-5e44-489a-b408-d4f6ac1c07e2" /> |
| :--- |
| ▲與SqueezeNet相同的訓練設定(Teachable Machine) |

# 四、實驗結果
### SqueezeNet程式碼：
```
 I = imread("test.jpg");  讀入圖片，注意檔案格式
I = imresize(I, [227 227]);  將圖片大小重新調整成規定尺寸
[YPred,probs] = classify(trainedNetwork_1,I);  神經網路判斷該圖片
imshow(I)
label = YPred;
title(string(label) + ", " + num2str(100*max(probs),3) + "%");
```
## Teachable Machine訓練：
以相同的訓練設定、訓練率下用Teachable Machine訓練出的成果：
| <img width="595" height="383" alt="image" src="https://github.com/user-attachments/assets/8fd7873b-5d4b-4e58-9563-77034365c43e" /> |
| :--- |
| ▲用Teachable Machine在相同的訓練條件下的訓練結果 |

# 五、實驗結果討論
| <img width="501" height="488" alt="image" src="https://github.com/user-attachments/assets/f6f940c3-68d5-41c3-8433-91022e3a3d49" /> |
| :--- |
| ▲用各類別的圖片測試的結果(SqueezeNet) |


| <img width="422" height="607" alt="image" src="https://github.com/user-attachments/assets/e2d0413c-578f-4050-896f-350b999f2758" /> |
| :--- |
| ▲用各類別的圖片測試的結果(Teachable Machine) |


在比對之下，會發現同樣的訓練率及其他設定下，SqueezeNet的正確率來到了99.5%以上，而Teachable Machine則是在90%上下，然而Teachable Machine背後不是單個模型，就精確度來講理論上來說會比追求極致輕量壓縮體積，大量減少參數的SqueezeNet高一點。因此我認為可能是涉及到資料庫的大小、以及辨識圖的選定，可能會導致兩個模型在特徵取樣上出現差異。

# 六、心得感想
這是我首次實際去學習深度學習及人工網路的應用還有實際操作，在操作上或許還有些許不夠精確的步驟，導致預期結果跟實際結果有些出入，未來我希望能夠更深入地去探討各個神經網路背後的參數、模型上的差異，以及準確度差異的背後原因，來更了解在大數據時代下神經網路的各種面向。











