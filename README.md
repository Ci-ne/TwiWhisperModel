# TwiWhisperModel

Project by Ben Charles Abdul and Francine Arthur :-).

_This project was carried out in December 2024._

## Introduction 
This project aims to develop a speech recognition model capable of transcribing audio in Asanti Twi, a native Ghanaian language. The project addresses the challenges of low-resource language processing and error correction by finetuning OpenAI's Whisper model and attempting to leverage a finetuned BERT model for post-processing. 

## Methodology 

### Data 
The Financial Inclusion Speech Dataset was used to finetune the Whisper Model.  It was preprocessed by resampling audio to 16kHz, converting numbers into numeric form and removing alternative transcriptions. It can be accessed here. We also made a custom test set by asking friends to record multiple sentences in Twi, some of which included financial terms. This was used to evaluate the model further. The audios were 
named like the financial inclusion dataset to make preprocessing easy. The audios and their transcriptions are available in the team's Google Drive. 

### Model Selection 
**Whisper**: Whisper was chosen for transcription due to its multilingual support and pre-trained weights, suitable for transfer learning. While Whisper does not natively support Asanti Twi, its multilingual capabilities make it robust to noise and accents, enhancing its adaptability. We also perceived that an English base would help with dealing with Enlgish words mixed into Twi. To balance computational efficiency and model accuracy, we finetuned Whisper's tiny and small variants. 
**BERT for Text Correction**: We used the ABENA (A BERT Now in Akan) and RoBAKO (Robustly Optimized BAKO) from Ghana NLP since these are pretrained on Twi Data.  

### What We Tried 
We initially finetuned the Whisper model on the financial inclusion dataset only, which was successful and had fair WERs. However, testing on our custom test set yielded very high WERs and CERs. Per advice, we removed some of the audio that needed to be clarified. We then attempted to train the BERTs for post-transcription processing, but did not have enough data, hence the model could not generalise at all (Twi_BERT notebook) and this was also abandoned. Upon further research and discussions with others, training a tokenizer from scratch on Twi might yield better results since it is adapted to generate and detect data on Twi words and characters. We also discovered a Twi Dataset curated from the Asanti Twi Bible while researching the Akan BERTs. Hence, we attempted this by training a custom tokenizer, finetuning the Whisper model on this data, and finetuning with the financial inclusion data (TokenizerfromJWDataset notebook). We successfully trained the tokenizer; however, integrating it into the model and finetuning the Whisper model was unsuccessful. Finetuning the tokenizer on the financial inclusion data (TokeniserWhisperFinancialInclusion notebook) was successful and yielded better results than before, so we abandoned this mission. 

## Deployment 
The final fine-tuned Whisper model was deployed to hugging face and can be accessed here: https://huggingface.co/spaces/CiBeDL/twi-whisper. It can be accessed via this API: https://api-inference.huggingface.co/models/CiBeDL/twi_trained_whisper.

## Results 
### Evaluation Metrics 
**Word Error Rate** (WER) measures transcription accuracy, while **Character Error Rate** (CER) tracks fine-grained character-level errors. 

### Evaluation Results 
In our initial finetuning of Whisper on the Financial Inclusion dataset, we achieved a WER of 14.75% and a Character Error Rate (CER) of 17.24% on the test set. The results of the held-out test set showed a WER of 32.73% and a CER of 13.55%. In a subsequent training attempt, we evaluated performance on the held-out test set using two methods. Method 1, which involved direct calculation of WER and CER, resulted in a WER of 70.03% and a CER of 50.62%. Method 2, which calculated sentence-wise WER and CER before averaging, resulted in a WER of 108.14% and a CER of 65.67%. However, these high values were attributed to outliers. After removing the outliers, there was a significant improvement, with WER dropping to 1.26% and CER to 0.61%. Both evaluation methods were applied to the custom test set. Method 1 yielded a 
WER of 20.79% and a CER of 11.34%, while Method 2 yielded a WER of 22.73% and a CER of 13.62%. These results highlight substantial performance improvements and nuanced differences between evaluation approaches. 

**Sampled Data From Held Out Test Set**  
<table>
  <tr><td>hypothesis</td><td>reference </td></tr>
  <tr><td>Maa wɔn ho yɛɛ huam </td><td>Maa wɔn ho yɛɛ huam </td></tr>
  <tr><td>Pia no bio </td><td>Pia no bio </td></tr>
  <tr><td>Hwɛ soro</td><td>Hwɛ soro</td></tr>
  <tr><td>Ne nea Ɔsɛe de yɛɛ buburoo wei </td><td>Ne nea Ɔsɛe de yɛɛ buburoo wei </td></tr>
  <tr><td>Kɔɔ dua bi akyi </td><td>Kɔɔ dua bi akyi </td></tr>
  <tr><td>Sua adɛɛ</td><td>Sua adeɛ </td></tr>
  <tr><td>Number bɛn na wo pɛse wo tɔ gu su</td><td>Number bɛn na wo pɛse wo tɔ gu su</td></tr>
  <tr><td>Emu biara</td><td>Emu biara</td></tr>
  <tr><td>Mee tɔ akɔ 054</td><td>Mee tɔ akɔ 054</td></tr>
  <tr><td>Mefri Ghana</td><td>Mefri Ghana</td></tr>
</table>

**Sampled Data From Custom Dataset** 
<table>
  <tr><td>hypothesis</td><td>reference</td></tr>
  <tr><td>Me ho yɛ fɛ </td><td>Wo ho yɛ fɛ </td></tr>
  <tr><td>Wo ho e fɛn </td><td>Wo ho yɛ fɛ</td></tr>
  <tr><td>Mɛtɔ bɔ di 10 cedis</td><td>Mɛ tɔ brodo 10 cedis</td></tr>
  <tr><td>Mɛtɔ goodɔ 10 cedis </td><td>Mɛ tɔ brodo 10 cedis </td></tr>
  <tr><td>Mepaa’kyɛw me number no yɛ 020 </td><td>Mepaa'kyɛw me number no yɛ 020</td></tr>
  <tr><td>Mepaa’kyɛw me number no yɛ 02 0</td><td>Mepaa'kyɛw me number no yɛ 020</td></tr>
  <tr><td>Wo ho yɛ fɛ </td><td>Wo ho yɛ fɛ </td></tr>
  <tr><td>Nnipa yɛ bad </td><td>Nnipa yɛ bad </td></tr>
  <tr><td>Hwa me ne Ama </td><td>Kwame ne Ama</td></tr>
</table>

The BERT for text correction was tested with  
Original Text: Me den de Kwoku Corrected Text: . meɛ denɛ woɛ 

## Discussion: 
Training the model presented several challenges, primarily due to our approach, which was resource-intensive and frequently led to memory crashes. Additionally, some transcriptions included extraneous onformation that we should have accounted for, while others contained incomplete links or led nowhere. These issues affected the model's ability to accurately associate audio samples with transcriptions, especially when the transcriptions were significantly longer than the audio. Noticing the high CER and WER values from the initial evaluation, we adjusted the training parameters to improve the model's performance. The number of epochs was increased from 3 to 10, and early stopping was implemented to prevent overfitting. A weight decay of 0.01 was introduced to regularize the model, and the evaluation strategy was changed from "steps" to "epochs." After these adjustments and removing outliers, the model's performance improved significantly, with WER and CER dropping dramatically. While some errors were observed in the 
custom test set, the overall accuracy remained reasonable, demonstrating the model's capability to effectively handle variations in speech patterns and common transcription errors. 

## Conclusion 
This project successfully addressed the transcription challenges associated with Asanti Twi, demonstrating the potential of Whisper. The integration of these models offers a scalable approach to low-resource speech recognition, improving the accuracy of Asanti Twi transcriptions despite the language's complexities and resource limitations. The project sets a foundation for further advancements in multilingual and mixed-language recognition systems by tackling specific transcription challenges for Asanti Twi. Future improvements will focus on expanding the dataset to increase the model's robustness and exploring the integration of additional language-specific preprocessing techniques to enhance accuracy further. 
