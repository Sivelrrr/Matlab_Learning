# eeg_htpCalcFooof Function
## Overview
The `eeg_htpCalcFooof` function performs power spectral analysis using the FOOOF (Fitting Oscillations & One-Over-F) approach. It decomposes EEG power spectra into periodic (oscillatory) and aperiodic (background) components, providing insights into underlying neural oscillations. This can be especially useful for analyzing the spectral content of EEG data in terms of background noise versus specific oscillatory peaks.

## Function Syntax
```
[EEG, results] = eeg_htpCalcFooof(EEG, varargin)
```

## Inputs

### Required Paramters
`EEG` : An EEGLAB structure containing EEG data. This structure should include EEG recordings, channel information, and sampling rate.

### Optional Parameters (Function-Specific)

Specify additional parameters through name-value pairs in varargin. Some key parameters include:

`f_range`: Frequency range of interest, typically defined as [`minFreq` `maxFreq`]. Defaults to [1 50].
`aperiodic_mode`: Method for estimating the aperiodic component (`fixed` or `knee`). Defaults to 'fixed'.
`peak_width_limits`: Limits on the width of detected peaks, as [`minWidth` `maxWidth`]. Defaults to [1 12].
`max_n_peaks`: Maximum number of peaks to model in the spectrum. Defaults to 6.
`plot_results`: Whether to plot FOOOF results (`true` or `false`). Defaults to `false`.



## Method Guide
### Load and Prepare EEG Data
Load your EEG data into an EEGLAB structure format. Ensure your data includes sampling rate and channel information required for spectral analysis.

```
EEG = pop_loadset('your_data.set'); % Load EEG data set
```

## Define Optional Parameters
Specify any optional parameters as name-value pairs. For instance, to set a frequency range from 1 to 40 Hz and plot results, you can set:

```
f_range = [1 40];
plot_results = true;
```
## Run eeg_htpCalcFooof

Execute the function with your EEG structure and any desired parameters:

```
[EEG, results] = eeg_htpCalcFooof(EEG, 'f_range', f_range, 'plot_results', plot_results);
```

This function will automatically analyze the EEG data across the specified frequency range and plot the results if plot_results is set to true.

Example Usage
This example demonstrates using eeg_htpCalcFooof with a specific frequency range, peak width limits, and plotting option:

```
% Define parameters
f_range = [1 40];
peak_width_limits = [2 10];
plot_results = true;

% Run FOOOF analysis
[EEG, results] = eeg_htpCalcFooof(EEG, 'f_range', f_range, ...
    'peak_width_limits', peak_width_limits, 'plot_results', plot_results);
```
## Outputs
`EEG` : Modified EEGLAB structure with `.etc.fooof` field containing FOOOF results.

`results` : Structure containing FOOOF analysis results, including aperiodic parameters, peak parameters, and goodness-of-fit metrics.

The results structure provides the following key outputs:

`aperiodic_params`: Parameters describing the aperiodic component.
`peak_param`s: Frequency, power, and width of detected oscillatory peaks.
`r_squared`: Goodness-of-fit measure for the FOOOF model.
`error`: Mean squared error of the model.

Additionally, if plot_results is set to `true`, the function displays a visual of the decomposed spectrum with aperiodic and periodic components.

## Additional Notes
FOOOF Dependency: This function requires the `FOOOF` package, so ensure FOOOF is installed and accessible in MATLAB.
Spectral Range Limitations: The selected `f_range` should align with the sampling rate of your data to avoid aliasing issues.
