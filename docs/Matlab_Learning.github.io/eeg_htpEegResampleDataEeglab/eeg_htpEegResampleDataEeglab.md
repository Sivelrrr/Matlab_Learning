# eeg_htpEegResampleDataEeglab Function

The eeg_htpEegResampleDataEeglab function resamples EEG data to a newly specified sampling rate using the EEGLAB toolbox. This guide will walk you through how to use this function effectively.

##Function Description
The eeg_htpEegResampleDataEeglab function takes an **EEGLAB** EEG structure and resamples its data to a new specified sampling rate. It optionally saves the output and logs relevant information.

### Syntax
```
[EEG, results] = eeg_htpEegResampleDataEeglab(EEG, 'srate', 500, 'saveoutput', false, 'outputdir', '')
```
## Inputs
### Required Parameter

`EEG`: An EEGLAB structure containing the EEG data.

### Optional Parameters

`srate`: A numeric value specifying the new sampling rate (default: 500).

`saveoutput`: A boolean indicating if the output should be saved (default: false).

`outputdir`: A string specifying the directory to save the output (default: '').

## Outputs

`EEG`: The updated **EEGLAB** structure with the resampled data.

`results`: A structure containing function-specific results, including a quality information (QI) table and input parameters used.

##Example Method
### 1. Load Your EEG Data
Before using the function, load your EEG data into an **EEGLAB** structure.
```
EEG = pop_loadset('filename', 'your_eeg_data.set', 'filepath', 'path_to_your_data/');
```
### 2. Resample Your EEG Data
Use the `eeg_htpEegResampleDataEeglab` function to resample your EEG data. You can specify the new sampling rate, whether to save the output, and the output directory.

Example: Resampling to 500 Hz and saving the output to a specified directory

```
[EEG, results] = eeg_htpEegResampleDataEeglab(EEG, 'srate', 500, 'saveoutput', true, 'outputdir', 'example_directory/');
```
### 3. Check the Results
After resampling, you can check the updated EEG structure and results.

Example: Displaying the new sampling rate
```
disp(EEG.srate);  % Should display the new sampling rate
```
Example: Viewing the QI table

```
disp(results.qi_table);
```

##Final Notes
Ensure that the **EEGLAB** toolbox is installed and added to your MATLAB path.
The function logs the original and new sampling rates and updates the EEGLAB structure accordingly.
If the original sampling rate is above 2000 Hz, it first downsamples to 1000 Hz before resampling to the specified rate.

