# dcase2024\_task2\_evaluator
The **dcase2024\_task2\_evaluator** is a script for calculating the AUC, pAUC, precision, recall, and F1 scores from the anomaly score list for the [evaluation dataset](https://zenodo.org/records/11363076) in DCASE 2024 Challenge Task 2 "First-Shot Unsupervised Anomalous Sound Detection for Machine Condition Monitoring."

[https://dcase.community/challenge2024/task-first-shot-unsupervised-anomalous-sound-detection-for-machine-condition-monitoring](https://dcase.community/challenge2024/task-first-shot-unsupervised-anomalous-sound-detection-for-machine-condition-monitoring)

## Description

The **dcase2024\_task2\_evaluator** consists of two scripts:

- `dcase2024_task2_evaluator.py`
    - This script outputs the AUC and pAUC scores by using:
      - Ground truth of the normal and anomaly labels
      - Anomaly scores for each wave file listed in the csv file for each machine type, section, and domain
      - Detection results for each wave file listed in the csv file for each machine type, section, and domain
- `03_evaluation_eval_data.sh`
    - This script execute `dcase2024_task2_evaluator.py`.

## Usage
### 1. Clone repository
Clone this repository from Github.

### 2. Prepare data
- Anomaly scores
    - Generate csv files `anomaly_score_<machine_type>_section_<section_index>_test.csv` and `decision_result_<machine_type>_section_<section_index>_test.csv` or `anomaly_score_DCASE2024T2<machine_type>_section_<section>_test_seed<seed><tag>_Eval.csv` and `decision_result_DCASE2024T2<machine_type>_section_<section>_test_seed<seed><tag>_Eval.csv` by using a system for the [evaluation dataset](https://zenodo.org/record/11363076). (The format information is described [here](https://dcase.community/challenge2024/task-first-shot-unsupervised-anomalous-sound-detection-for-machine-condition-monitoring#submission).)
- Rename the directory containing the csv files to a team name
- Move the directory into `./teams/`

### 3. Check directory structure
- ./dcase2024\_task2\_evaluator
    - /dcase2024\_task2\_evaluator.py
    - /03\_evaluation\_eval\_data.sh
    - /ground\_truth\_attributes
        - ground\_truth\_3DPrinter\_section\_00\_test.csv
        - ground\_truth\_AirCompressor\_section\_00\_test.csv
        - ...
    - /ground\_truth\_data
        - ground\_truth\_3DPrinter\_section\_00\_test.csv
        - ground\_truth\_AirCompressor\section\_00\_test.csv
        - ...
    - /ground\_truth\_domain
        - ground\_truth\_3DPrinter\_section\_00\_test.csv
        - ground\_truth\_AirCompressor\_section\_00\_test.csv
        - ...
    - /teams
        - /\<team\_name\_1\>
            - /\<system\_name\_1\>
                - anomaly\_score\_3DPrinter\_section\_00\_test.csv
                - anomaly\_score\_AirCompressor\_section\_00\_test.csv
                - ...
                - decision\_result\_ToothBrush\_section\_00\_test.csv
                - decision\_result\_ToyCircuit\_section\_00\_test.csv
            - /\<system\_name\_2\>
                - anomaly\_score\_DCASE2024T23DPrinter\_section\_00\_test\_seed\<--seed\>\<--tag\>\_Eval.csv
                - anomaly\_score\_DCASE2024T2AirCompressor\_section\_00\_test\_seed\<--seed\>\<--tag\>\_Eval.csv
                - ...
                - decision\_result\_DCASE2024T2ToothBrush\_section\_00\_test\_seed\<--seed\>\<--tag\>\_Eval.csv
                - decision\_result\_DCASE2024T2ToyCircuit\_section\_00\_test\_seed\<--seed\>\<--tag\>\_Eval.csv
        - /\<team\_name\_2\>
            - /\<system\_name\_3\>
                - anomaly\_score\_3DPrinter\_section\_00\_test.csv
                - anomaly\_score\_AirCompressor\_section\_00\test.csv
                - ...
                - decision\_result\_ToothBrush\_section\_00\_test.csv
                - decision\_result\_ToyCircuit\_section\_00\_test.csv
        - ...
    - /teams\_result
        - \<system\_name\_1\>\_result.csv
        - \<system\_name\_2\>\_result.csv
        - \<system\_name\_3\>_result.csv
        - ...
    - /teams\_additional\_result \*`out_all==True`
        - teams\_official\_score.csv
        - teams\_official\_score\_paper.csv
        - teams\_section\_00\_auc.csv
        - teams\_section\_00\_score.csv
        - /\<system\_name\_1\>
            - official\_score.csv
            - \<system\_name\_1\>\_3DPrinter\_section\_00\_anm\_score.png
            - ...
            - \<system\_name\_1\>\_ToyCircuit\_section\_00\_anm\_score.png
        - /\<system\_name\_2\>
            - official\_score.csv
            - \<system\_name\_2\>\_3DPrinter\_section\_00\_anm\_score.png
            - ...
            - \<system\_name\_2\>\_ToyCircuit\_section\_00\_anm\_score.png
        - /\<system\_name\_3\>
            - official\_score.csv
            - \<system\_name\_3\>\_3DPrinter\_section\_00\_anm\_score.png
            - ...
            - \<system\_name\_3\>\_ToyCircuit\_section\_00\_anm\_score.png
        - ...
    - /tools
        - plot\_anm\_score.py
        - test\_plots.py
    - /README.md


### 4. Change parameters
The parameters are defined in the script `dcase2024_task2_evaluator.py` as follows.
- **MAX\_FPR**
    - The FPR threshold for pAUC : default 0.1
- **--result\_dir**
    - The output directory : default `./teams_result/`
- **--teams\_root\_dir**
    - Directory containing team results. : default `./teams/`
- **--dir\_depth**
    - What depth to search `--teams_root_dir` using glob. : default `2`
    - If --dir\_depth=2, then `glob.glob(<teams_root_dir>/*/*)`
- **--tag**
    - File name tag. : default `_id(0_)`
    - If using filename is DCASE2024 baseline style, change parameters as necessary. 
- **--seed**
    - Seed used during train. : default `13711`
    - If using filename is DCASE2024 baseline style, change parameters as necessary.
- **--out\_all**
    - If this parameter is `True`, export supplemental data. : default `False`
- **--additional\_result\_dir**
    - The output additional results directory. : default `./teams_additional_result/`
    - Used when `--out_all==True`.

### 5. Run script
Run the script `dcase2024_task2_evaluator.py`
```
$ python dcase2024_task2_evaluator.py
```
or
```
$ bash 03_evaluation_eval_data.sh
```
The script `dcase2024_task2_evaluator.py` calculates the AUC, pAUC, precision, recall, and F1 scores for each machine type, section, and domain and output the calculated scores into the csv files (`<system_name_1>_result.csv`, `<system_name_2>_result.csv`, ...) in **--result\_dir** (default: `./teams_result/`).
If **--out\_all=True**, each team results are then aggregated into a csv file (`teams_official_score.csv`, `teams_official_score_paper.csv`) in **--additional\_result\_dir** (default: `./teams_additional_result`).

### 6. Check results
You can check the AUC, pAUC, precision, recall, and F1 scores in the `<system_name_N>_result.csv` in **--result\_dir**.
The AUC, pAUC, precision, recall, and F1 scores for each machine type, section, and domain are listed as follows:

`<section_name_N>_result.csv`
```
3DPrinter
section,AUC (all),AUC (source),AUC (target),pAUC,precision (source),precision (target),recall (source),recall (target),F1 score (source),F1 score (target)
00,0.6354,0.7912,0.47959999999999997,0.4942105263157895,0.717391304347826,0.5180722891566265,0.66,0.86,0.6875,0.6466165413533834
,,AUC,pAUC,precision,recall,F1 score
arithmetic mean,,0.6354,0.4942105263157895,0.6177317967522262,0.76,0.6670582706766917
harmonic mean,,0.5971978596159899,0.49421052631578954,0.6016535933856264,0.746842105263158,0.666431842197957
source harmonic mean,,0.7912,0.49421052631578954,0.717391304347826,0.66,0.6875
target harmonic mean,,0.47959999999999997,0.49421052631578954,0.5180722891566265,0.86,0.6466165413533834

...

ToyCircuit
section,AUC (all),AUC (source),AUC (target),pAUC,precision (source),precision (target),recall (source),recall (target),F1 score (source),F1 score (target)
00,0.6462,0.7658,0.5266000000000001,0.5,0.6301369863013698,0.5,0.92,1.0,0.7479674796747968,0.6666666666666666
,,AUC,pAUC,precision,recall,F1 score
arithmetic mean,,0.6462000000000001,0.5,0.5650684931506849,0.96,0.7073170731707317
harmonic mean,,0.6240641906530486,0.5,0.5575757575757575,0.9583333333333334,0.7049808429118775
source harmonic mean,,0.7658,0.5,0.6301369863013698,0.92,0.7479674796747968
target harmonic mean,,0.5266000000000001,0.5,0.5,1.0,0.6666666666666666

...

,,AUC,pAUC,precision,recall,F1 score
"arithmetic mean over all machine types, sections, and domains",,0.6204444444444445,0.5181286549707603,0.5464815273010529,0.8455555555555555,0.6467236490381916
"harmonic mean over all machine types, sections, and domains",,0.592492919258281,0.5172271108149132,0.538728700879929,0.7936853315864981,0.6418141165949086
"source harmonic mean over all machine types, sections, and domains",,0.7150555228086642,0.5172271108149132,0.5725675329945789,0.7068231605837113,0.6326511445646708
"target harmonic mean over all machine types, sections, and domains",,0.5057977147441246,0.5172271108149132,0.5086664338700246,0.9048878623155235,0.6512464122791479

official score,,0.5650830191796572
official score ci95,,1.050516938082758e-05
```

Aggregated results for each baseline are listed as follows:

```_seed13711_official_score_paper.csv
System,metric,h-mean,a-mean,3DPrinter,AirCompressor,Scanner,ToyCircuit,HoveringDrone,HairDryer,ToothBrush,RoboticArm,BrushlessMotor
DCASE2024_baseline_task2_MAHALA,AUC (source),0.6718221907158789,0.6833555555555556,0.7544,0.6609999999999999,0.5684,0.6794,0.8387999999999999,0.606,0.6556,0.5834,0.8032
DCASE2024_baseline_task2_MAHALA,AUC (target),0.5144113485475758,0.5302222222222222,0.42399999999999993,0.555,0.5549999999999999,0.4464,0.46399999999999997,0.5898,0.579,0.42499999999999993,0.7338
DCASE2024_baseline_task2_MAHALA,"pAUC (source, target)",0.5324024369331867,0.5360233918128655,0.508421052631579,0.6078947368421053,0.4926315789473684,0.4921052631578947,0.5800000000000001,0.5510526315789473,0.48368421052631577,0.5178947368421053,0.5905263157894737
DCASE2024_baseline_task2_MAHALA,TOTAL score,0.5648933421610234,0.5832003898635478,,,,,,,,,
DCASE2024_baseline_task2_MSE,AUC (source),0.7150555228086642,0.7220222222222223,0.7912,0.65,0.6916,0.7658,0.8601999999999999,0.6224000000000001,0.7504000000000001,0.6596,0.7070000000000001
DCASE2024_baseline_task2_MSE,AUC (target),0.5057977147441246,0.5188666666666667,0.47959999999999997,0.4824,0.5374000000000001,0.5266000000000001,0.43879999999999997,0.4598,0.6948,0.4152,0.6352
DCASE2024_baseline_task2_MSE,"pAUC (source, target)",0.5172271108149132,0.5181286549707603,0.4942105263157895,0.5547368421052632,0.5010526315789474,0.5,0.5021052631578947,0.5163157894736842,0.5273684210526316,0.511578947368421,0.5557894736842105
DCASE2024_baseline_task2_MSE,TOTAL score,0.5650830191796572,0.5863391812865497,,,,,,,,,

```

## Change log
### 2024/09/10 Added Ground truth attributes

- Attributes have been added to following machine types for which Ground Truth had "noAttribute".
  - [Development dataset](ground_truth_attributes_all/dev_data)
    - train data
    - test data
  - [Evaluation dataset](ground_truth_attributes_all/eval_data)
    - train data
    - test data

### 2024/08/02 Added Ground truth attributes

- Attributes have been added to three machine types for which Ground Truth had "noAttribute".
    - AirCompressor
    - HoveringDrone
    - ToothBrush

## Citation

If you use this system, please cite all the following four papers:

+ Tomoya Nishida, Noboru Harada, Daisuke Niizumi, Davide Albertini, Roberto Sannino, Simone Pradolini, Filippo Augusti, Keisuke Imoto, Kota Dohi, Harsh Purohit, Takashi Endo, and Yohei Kawaguchi. Description and discussion on DCASE 2024 challenge task 2: first-shot unsupervised anomalous sound detection for machine condition monitoring. In arXiv e-prints: 2406.07250, 2024. [URL](https://arxiv.org/pdf/2406.07250.pdf)
+ Noboru Harada, Daisuke Niizumi, Daiki Takeuchi, Yasunori Ohishi, Masahiro Yasuda, and Shoichiro Saito. ToyADMOS2: another dataset of miniature-machine operating sounds for anomalous sound detection under domain shift conditions. In Proceedings of the Detection and Classification of Acoustic Scenes and Events Workshop (DCASE), 1–5. Barcelona, Spain, November 2021. [URL](https://dcase.community/documents/workshop2021/proceedings/DCASE2021Workshop_Harada_6.pdf)
+ Kota Dohi, Tomoya Nishida, Harsh Purohit, Ryo Tanabe, Takashi Endo, Masaaki Yamamoto, Yuki Nikaido, and Yohei Kawaguchi. MIMII DG: sound dataset for malfunctioning industrial machine investigation and inspection for domain generalization task. In Proceedings of the 7th Detection and Classification of Acoustic Scenes and Events 2022 Workshop (DCASE2022). Nancy, France, November 2022. [URL](https://dcase.community/documents/workshop2022/proceedings/DCASE2022Workshop_Dohi_62.pdf)
+ Noboru Harada, Daisuke Niizumi, Daiki Takeuchi, Yasunori Ohishi, and Masahiro Yasuda. First-shot anomaly detection for machine condition monitoring: a domain generalization baseline. Proceedings of 31st European Signal Processing Conference (EUSIPCO), pages 191–195, 2023. [URL](https://eurasip.org/Proceedings/Eusipco/Eusipco2023/pdfs/0000191.pdf)