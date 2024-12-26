# TwiWhisperModel

Project by Ben Charles Abdul and Francine Arthur :-).


This project aimed to finetune the Whsiper model by OpenAI for transcribing Twi Audios. We experimented quite a lot of different things - first finetuning on just the audio and transcriptions available from the Ashesi Financial Inclusion Dataset (https://github.com/Ashesi-Org/Financial-Inclusion-Speech-Dataset), then finetuning it on a custom datset. We also tried making a custom tokeniser, first from the same financial inclusion dataset, then from the Twi Bible which we found on huggingface (https://huggingface.co/datasets/kojo-george/asante-twi-tts). Lastly, we wanted to use BERTs for text correction to correct any errors that the transcription might generate (but that was never really worked out well).

Finally we deployed it on huggingface. You can try it out here: https://huggingface.co/spaces/CiBeDL/twi-whisper.

_This project was carried out in December 2024._
