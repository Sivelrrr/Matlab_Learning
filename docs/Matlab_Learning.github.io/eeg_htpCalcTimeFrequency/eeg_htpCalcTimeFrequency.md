## Overview
The `eeg_htpCalcTimeFrequency` function calculates time-frequency analyses, particularly inter-trial coherence (ITC) and event-related spectral perturbation (ERSP), to assess frequency-specific power and phase-locking in EEG data. It's built for auditory stimuli experiments but can be adapted to other paradigms.

Syntax
```
[EEG, results] = eeg_htpCalcTimeFrequency(EEG, 'parameterName', parameterValue, ...)
```
## Required Input
`EEG`: Main input of the function and a structure with fields such as `.data` (EEG signal), `.srate` (sampling rate), `.chanlocs` (channel info), and `.pnts` (number of points).

## Optional Parameters

### General Parameters
`tlimits` (default: [-500 2750]): Defines the time range for the analysis in milliseconds. Adjust this range to capture the complete ERP response.

`flimits` (default: [2 110]): Specifies the frequency range in Hz for the analysis. The default captures both low and high frequencies.

`cycles` (default: [1 30]): Wavelet cycles, represented as a two-element vector, where higher values improve frequency resolution but increase computation time.

`timesout` (default: 250): The number of time points for the output. Increasing this parameter provides finer temporal resolution.

`winsize` (default: 100): Size of the moving window in milliseconds for calculating the time-frequency response.

`nfreqs` (default: 109): The number of frequencies for time-frequency decomposition.

`outputdir` (default: `tempdir`): Directory for saving result files. Adjust this to store outputs in a specific project folder.

### Artifact Rejection
`ampThreshold` (default: 120): Threshold for artifact rejection. Trials exceeding this amplitude are excluded from analysis.

`emptyEEG` (default: `true`): Removes EEG data from the output structure to save memory, retaining only summary metrics.

### Data Specific Options

`sourceOn` (default: `false`): When `true`, the function selects electrodes specifically for source analysis targeting auditory cortex regions.

`byChannel` (default: `false`): When set to `true`, the function computes time-frequency data for each individual channel. When `false`, it averages signals from selected sensors.

`baselinew` (default: [-500 0]): A time range in milliseconds for ERSP baseline correction. This baseline is applied to normalize ERSP data relative to a pre-stimulus period.

## Example Usage
Here is a practical example for running this function with baseline correction and source-based electrode selection:

```
[EEG, results] = eeg_htpCalcTimeFrequency(EEG, ...
                                          'sourceOn', true, ...
                                          'byChannel', true, ...
                                          'baselinew', [-500 0]);
```
In this setup:

`sourceOn` = if `true` uses specific electrodes within the auditory cortex.

`byChannel` = if `true` processes each selected channel individually.

`baselinew` = [-500 0] applies baseline correction for ERSP based on the -500 to 0 ms pre-stimulus window.

## Outputs
`EEG`: Modified EEGLAB EEG structure with additional fields for time-frequency data in `EEG.vhtp`.

`results`: A structure containing:

- ITC and ERSP data across channels

- Summary table of results, including metrics like mean power and phase coherence over defined ROIs.

- Rejection statistics for artifact-prone epochs.

Example Workflow

Load EEG data and ensure it is epoched for the stimulus of interest.
Define analysis parameters:

- Set `tlimits` and `flimits` for the specific time and frequency ranges relevant to your study.

- Define cycles and timesout based on desired frequency and temporal resolution.

Run artifact rejection using `ampThreshold` to exclude noisy epochs.

Run the function with necessary parameters, as shown in the example above.

Analyze and save results:

 - Examine the `results` structure for summary tables and ITC/ERSP data.

Save output tables to the specified directory for report generation.

## Additional Notes
Artifact rejection is performed by checking if any trial exceeds the amplitude threshold defined by `ampThreshold`.

ROI specification allows targeted frequency and time ranges for specific ERP components, particularly useful for auditory or visual stimuli.

Further customization is possible by modifying `cycles`, `nfreqs`, and baseline parameters based on experiment design.

### Troubleshooting
Low trial count error: Ensure that EEG data contains at least 10 trials for stable results.

High computation time: Increase `winsize` and reduce `nfreqs` if computation is slow, especially for high-density data.