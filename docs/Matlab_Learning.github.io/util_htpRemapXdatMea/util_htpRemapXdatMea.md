# util_htpRemapXdatMea Function

## Overview
The `util_htpRemapXdatMea` function is designed to reorder or remap EEG channel data within an EEGLAB EEG structure according to a specified mapping. This function is particularly useful if the recorded data channels do not correspond to the correct physical electrode locations and need to be reassigned based on a predefined mapping scheme.

## Function Syntax

```
EEG = util_htpRemapXdatMea(EEG)
```

## Inputs

`EEG`: An EEGLAB structure containing the EEG data to be remapped. The EEG.data field should contain EEG signals in a 2D array format, where each row represents a channel, and each column represents a time point.


## Methods
## Load EEG Data

Load your EEG data as an EEGLAB structure with channels in the existing order.

```
EEG = pop_loadset('your_data.set');  % Load EEG data set
```

## Running `util_htpRemapXdatMea`
Execute the function to remap the channels within the EEG structure based on the predefined dictionary.

```
EEG = util_htpRemapXdatMea(EEG);
```

## Mapping 
The function remaps channels from their current order to a new, "correct" order using a dictionary (`mappingDict`). This dictionary maps each original channel index (from 1 to 30) to a new index, where each entry represents a correct electrode location for EEG data.

For example:

Channel 1 in the original data is mapped to position 29 in the reordered data.

Channel 2 in the original data is mapped to position 27 in the reordered data.

These mappings continue for all channels up to 30. The `reorderedArray` is filled according to this mapping, and the original `EEG.data` is replaced with the reordered data.

## Verify Remapping Results
You can verify the remapping by inspecting `EEG.data` to ensure channels are in the expected positions according to the mapping scheme.

```
disp(EEG.data);  % Display the remapped EEG data

```
## Full Example
```
% Load EEG data
EEG = pop_loadset('your_data.set');

% Remap channels using predefined mapping
EEG = util_htpRemapXdatMea(EEG);

% Verify remapped channels
disp(EEG.data);  % Check the reordered data
```
## Output
`EEG`: The input EEGLAB structure with its `.data` field remapped according to the specified `mappingDict` dictionary.


## Additional Notes
Mapping Dictionary: Adjustments to the mapping dictionary may be necessary if the channel configuration changes or if different data is used.

Data Consistency: Be cautious when applying this remapping to ensure it corresponds accurately with the physical setup used during data collection.