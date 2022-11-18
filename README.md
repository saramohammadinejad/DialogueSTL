# DialogueSTL
Explainable and Interactive Learning from Natural Language and Demonstrations using Signal Temporal Logic (STL)

Demo of DialogueSTL: https://youtu.be/C2EoxxxKkrA

This repository describes the steps for repeating the experiments of DialogueSTL tool:

If you face any issues please contact ```saramoha@usc.edu```

--------------------------------------------------------------------------
Step 1) Python setup
<br />
&emsp;Step 1.1) Install ```Python 3.8.6```

&emsp;Step 1.2) Create a python virtual environment and activate it using the following commands:

  ```ruby
  python3.8.6 -m venv my_venv

  source my_venv/bin/activate
  ```

&emsp;Step 1.3) install the required tools, packages and libraries listed in requirements.txt
<br />
<br />
Step 2) Matlab Setup
<br />
&emsp;Step 2.1) Install MATLAB
  
&emsp;Step 2.2) Install Breach on MATLAB

&emsp;https://github.com/decyphir/breach (We use Breach for computing the robustness of STL formulas)
  
&emsp;Step 2.3) Install MATLAB Engine API for Python https://www.mathworks.com/help/matlab/matlab_external/get-started-with-matlab-engine-for-python.html

<br />
<br />
step 3) Clone the repository and unzip it
<br />
<br />
step 4) Go to directory ```DialogueSTL_RE_package/rasa_atom_classifier```
<br />
<br />
step 5) train the atom classifier using the following command:

```ruby
rasa train nlu
```

It takes less than 1 minute for the model to be trained. The trained model will be saved in ```DialogueSTL_RE_package/rasa_atom_classifier/models``` folder. You have to copy its name and replace it in line 33 of the file ```DialogueSTL_RE_package/main_interaction_experiments.py```

Next, you can test the atom classifier by running 

```ruby
rasa test nlu
```

The test results will be saved in folder ```DialogueSTL_RE_package/rasa_atom_classifier/results```
<br />
<br />
step 6) run:


```ruby
python main_interaction_experiments.py
```
--------------------------------------------------------------------------
Comparison with DeepSTL:

The link to DeepSTL paper is: https://arxiv.org/pdf/2109.10294.pdf

The code for DeepSTL paper can be downloaded from one the following links:

https://github.com/JieHE-2020/DeepSTL

https://figshare.com/articles/software/Artifact_of_Paper_DeepSTL_-_From_English_Requirements_to_Signal_Temporal_Logic_/19091282

There is a detailed document on how to repeat the experiments for DeepSTL. When you download the package using one of the above links, the detailed document is provided in documents/README.pdf. Please follow the steps for complete track. Before running the package you need to generate data for that (to make a comparison with our tool).

Step 6.1) Run data_generate_from_dialoguestl.py to generate training data for DeepSTL (make sure to modify the path included in this file in line 173 to your path) 

Step 6.2) Make sure the corpus_split.csv in directory "DeepSTL-main/Archive-4/material/artifact/complete_track/NLP/data_preprocessing/subword/dataset/corpus_split.csv" is updated with the generated data. There should be 252866 instances of data in corpus_split.csv. But we want to randomly select 120000 instances among them. Open the file subword_preprocessing and add this block of code after line 48. 

```ruby
total_data_size = 120000
eng_data_list = eng_data_list[:total_data_size]
stl_data_list = stl_data_list[:total_data_size]
```

Step 6.3) Go to folder NLP and run:

```ruby
python subword_preprocessing.py
```

Step 6.4) Start training using the command: 

```ruby
python transformer_train.py 
```

Step 6.5) Start testing using: 

```ruby
python transformer_test.py
```

There are three modes for running transformer_test.py:

```ruby
'inner_test', 
'extrapolate', 
'output_results' 
```

If you choose 'inner_test' it will test the tool on 10% of the data that has been splitted from train set. If you choose 'extrapolate' it will test the tool on data provided in ```NLP/test_cases/test_case_stl.txt``` and compares the results with the ground truth provided in NLP/test_cases/test_case_eng.txt. So before running this mode you need to update these two files with our GPT-3 generated data. You can find our GPT-3 generated data in "nl" field of the file "DialogueSTL_RE_Package/json_test_files/tests.json"

