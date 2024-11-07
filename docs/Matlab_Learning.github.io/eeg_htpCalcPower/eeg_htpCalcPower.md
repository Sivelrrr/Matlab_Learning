# eeg_htpCalcPower Function

## Introduction
The `eeg_htpCalcRestPower` function calculates the spectral power of resting-state EEG data using the Welch method, which is a common approach for spectral analysis. Below is documentation on how to use this function effectively.

## Preparing The Data
Before using the function make sure [**EEGLAB**](https://eeglab.org/download/) is running otherwise the function will error

It is also necessary to first load in the EEG file and then extract the EEG strcuture as that is the required input for the funciton.
(**note** - make sure the EEG structure includes both a subject id and filename).

## Function Inputs

```
[EEG, results] = eeg_htpCalcRestPower(EEG, varargin)
```
The function shown above has multiple inputs: `EEG` and `Varargin`. 

`EEG`: Main input of the function and a structure with fields such as `.data` (EEG signal), `.srate` (sampling rate), `.chanlocs` (channel info), and `.pnts` (number of points).

Several optional parameters using the `varargin` to modify the analysis. You can pass  Below are some of the important parameters.

`gpuon` (logical): Use GPU acceleration to speed up computation. Default is false. Set to true if you have the MATLAB [Parallel-Processing](https://www.mathworks.com/products/parallel-computing.html) toolbox installed.

`duration` (integer): Duration in seconds to calculate power. Default is 80 seconds. If the duration exceeds the data length, it uses the maximum available data.

`offset` (integer): Time in seconds to start the calculation. Default is 0.

`bandDefs` (cell array): Frequency band definitions. You can specify bands like delta, theta, alpha, beta, gamma, etc. Example:

```
{'delta', 2, 3.5; 'theta', 3.5, 7.5; 'alpha1', 8, 10; 'beta', 13, 30}
```

`outputdir` (string): Directory to save the results. By default, it saves in the same directory as the EEG data.

`useParquet`(logical): Save the output in [Parquet](https://parquet.apache.org/docs/) format. Default is set to `false` and will save as CSV if `false`.

## Running the Function with Default Parameters

Here's a simple example using default settings:

```
[EEG, results] = eeg_htpCalcRestPower(EEG);
```
In this example:

The function will calculate spectral power for the default 80 seconds of EEG data.
It will save the results in CSV format in the default output directory, add a `.vhtp` structure to the `EEG` structure containing the same tables, and will create a structure stored in `results` with the same results for easier access.

## Customizing Analysis

In this case you want to utilize GPU acceleration and change the duration to 30 seconds. You can pass the parameters like this:


```
[EEG, results] = eeg_htpCalcRestPower(EEG, 'gpuon', true, 'duration', 30);
```
In this example:
GPU is enabled (key-value pair: `gpuon`, `true`).
The analysis is performed on 30 seconds of data (key-value pair: `duration` , 30).

## Outputs
Once the function runs, it will output two variables shown below:

```
[EEG, results] = eeg_htpCalcRestPower(EEG);
```

`EEG`: The input EEG structure with the added `.vhtp` field.

`results`: A structure with the spectral analysis results.

 It will also save the following data into separate files (based on your settings):


- Spectral power results for each channel and frequency band.

- Spectrogram data as a CSV (or Parquet if enabled) showing the power spectral density across channels.

- The Quality Index (QI) file summarizes information about the EEG dataset and analysis.

These files will be saved in the output directory specified by `outputdir`.

## Inspecting the Results

The results structure contains the following fields:

- `summary_table`: A table of power values (absolute, relative, and dB) for each frequency band.
- `spectro`: The computed spectrogram for each channel. It also includes relative, absolute, and decibel measurements.
- `qi_table`: Metadata about the analysis (e.g., sampling rate, number of channels).

Example of how to view the summary table:

```
disp(results.summary_table);
```
Will display a table similar to the following example (**Note** - this table is a shortened version of the full table):

```
  eegid           filename           chan      abs_delta    abs_theta    abs_alpha1    
    __________    _________________    ________    _________    _________    __________    
    "EGI file"    "0228_BBLong.raw"    {'E1'  }       28.541       17.488       23.088        
    "EGI file"    "0228_BBLong.raw"    {'E2'  }       17.461        13.49       21.089        

```
## Summary
Use **eeg_htpCalcRestPower** to calculate spectral power from EEG data.
Customize the analysis by adjusting parameters like `gpuon`, `duration`, and `bandDefs`.
Save results in either CSV or Parquet format.

