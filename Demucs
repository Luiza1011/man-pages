Demucs3
install demucs
python3 -m pip install -U demucs

In order to try Demucs, you can just run from any folder (as long as you properly installed it)

demucs PATH_TO_AUDIO_FILE_1 [PATH_TO_AUDIO_FILE_2 ...]   # for Demucs
# If you used `pip install --user` you might need to replace demucs with python3 -m demucs
python3 -m demucs --mp3 --mp3-bitrate BITRATE PATH_TO_AUDIO_FILE_1  # output files saved as MP3
# If your filename contain spaces don't forget to quote it !!!
demucs "my music/my favorite track.mp3"
# You can select different models with `-n` mdx_q is the quantized model, smaller but maybe a bit less accurate.
demucs -n mdx_q myfile.mp3
# If you only want to separate vocals out of an audio, use `--two-stems=vocal` (You can also set to drums or bass)
demucs --two-stems=vocals myfile.mp3

If you have a GPU, but you run out of memory, please use --segment SEGMENT to reduce length of each split. SEGMENT should be changed to a integer. Personally recommend not less than 10 (the bigger the number is, the more memory is required, but quality may increase). Create an environment variable PYTORCH_NO_CUDA_MEMORY_CACHING=1 is also helpful. If this still cannot help, please add -d cpu to the command line. See the section hereafter for more details on the memory requirements for GPU acceleration.

Separated tracks are stored in the separated/MODEL_NAME/TRACK_NAME folder. There you will find four stereo wav files sampled at 44.1 kHz: drums.wav, bass.wav, other.wav, vocals.wav (or .mp3 if you used the --mp3 option).

All audio formats supported by torchaudio can be processed (i.e. wav, mp3, flac, ogg/vorbis on Linux/Mac OS X etc.). On Windows, torchaudio has limited support, so we rely on ffmpeg, which should support pretty much anything. Audio is resampled on the fly if necessary. The output will be a wave file encoded as int16. You can save as float32 wav files with --float32, or 24 bits integer wav with --int24. You can pass --mp3 to save as mp3 instead, and set the bitrate with --mp3-bitrate (default is 320kbps).

It can happen that the output would need clipping, in particular due to some separation artifacts. Demucs will automatically rescale each output stem so as to avoid clipping. This can however break the relative volume between stems. If instead you prefer hard clipping, pass --clip-mode clamp. You can also try to reduce the volume of the input mixture before feeding it to Demucs.

Other pre-trained models can be selected with the -n flag. The list of pre-trained models is:

htdemucs: first version of Hybrid Transformer Demucs. Trained on MusDB + 800 songs. Default model.
htdemucs_ft: fine-tuned version of htdemucs, separation will take 4 times more time but might be a bit better. Same training set as htdemucs.
htdemucs_6s: 6 sources version of htdemucs, with piano and guitar being added as sources. Note that the piano source is not working great at the moment.
hdemucs_mmi: Hybrid Demucs v3, retrained on MusDB + 800 songs.
mdx: trained only on MusDB HQ, winning model on track A at the MDX challenge.
mdx_extra: trained with extra training data (including MusDB test set), ranked 2nd on the track B of the MDX challenge.
mdx_q, mdx_extra_q: quantized version of the previous models. Smaller download and storage but quality can be slightly worse.
SIG: where SIG is a single model from the model zoo.
The --two-stems=vocals option allows to separate vocals from the rest (e.g. karaoke mode). vocals can be changed into any source in the selected model. This will mix the files after separating the mix fully, so this won't be faster or use less memory.

The --shifts=SHIFTS performs multiple predictions with random shifts (a.k.a the shift trick) of the input and average them. This makes prediction SHIFTS times slower. Don't use it unless you have a GPU.

The --overlap option controls the amount of overlap between prediction windows. Default is 0.25 (i.e. 25%) which is probably fine. It can probably be reduced to 0.1 to improve a bit speed.

The -j flag allow to specify a number of parallel jobs (e.g. demucs -j 2 myfile.mp3). This will multiply by the same amount the RAM used so be careful!
