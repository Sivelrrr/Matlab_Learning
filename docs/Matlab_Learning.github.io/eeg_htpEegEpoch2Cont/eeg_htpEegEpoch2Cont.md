# eeg_htpEegEpoch2Cont Function

## Overview
This page provides documentation on how to use `eeg_htpEegEpoch2Cont` function designed to convert epoched EEG data into continuous data. This function is categorized under "Preprocessing" and helps streamline the transition from segmented to continuous data formats, which is essential for various EEG analyses.

## Function Description
The function takes epoched EEG data (data segmented into trials) and converts it into a continuous data format. This process involves reshaping the data matrix and updating relevant metadata fields such as the number of data points and the time vector.

## Function
```
function EEG = epoched_to_continuous(EEG)
```
## Inputs
`EEG`: A structure containing EEG data and associated metadata. This structure must have the following fields:

- `data`: A 3D matrix of EEG data with dimensions (channels x points x trials).
    
- `srate`: The sampling rate of the EEG data.

## Outputs
`EEG`: The modified EEG structure with continuous data. The relevant fields are updated to reflect the new data format.

## Detailed Steps
Input Validation: The function starts by validating the input using MATLAB's `inputParser` to ensure the input EEG is a structure.

Dimension Check: It checks if the EEG data has more than two dimensions, indicating that it is epoched.

Data Reshaping: If the data is epoched, it reshapes the 3D data matrix into a 2D matrix, effectively concatenating trials into a continuous stream.

Metadata Update: The function updates the number of points (`EEG.pnts`) and the time vector (`EEG.times`) based on the new continuous data format.

Data Conversion: It converts the data to `double` precision to ensure numerical stability.

Set Check: It performs a check on the `EEG` structure using `eeg_checkset` (a common function in EEG processing toolboxes like **EEGLAB**) to ensure the structure's consistency.

## Example Method

### 1. Load Your EEG Data
Before using the function, load your EEG data into an **EEGLAB** structure.

```
EEG = pop_loadset('filename', 'your_eeg_data.set', 'filepath', 'example_path/');
```
### 2. Convert Epoched Data to Continuous Data
Use the `eeg_htpEegEpoch2Cont` function to convert your epoched EEG data to continuous data.

Example: Converting epoched data to continuous data

```
EEG = eeg_htpEegEpoch2Cont(EEG);
```

### 3. Check the Results
After converting, you can check the updated EEG structure.

Example: Displaying the size of the continuous data

```
disp(size(EEG.data));  % Should display the new size of the continuous data
```

## Final Notes
Ensure that the `eeg_checkset` function is available in your MATLAB path. This function is typically part of the **EEGLAB** toolbox. The function assumes that the input EEG data is correctly formatted and that the `data` field contains a 3D matrix if epoched. If the data is already continuous then a warning will we displayed and no changes will occur.
