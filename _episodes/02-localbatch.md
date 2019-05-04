---
title: "Local Batch Processing"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

Final output file with comments
~~~
% Load files
EEG = pop_loadset('filename','[batch_dfn]','filepath','[batch_dfp]');
EEG = eeg_checkset( EEG );

% Rereference via average reference
EEG = pop_reref( EEG, []);
EEG.setname='avgref';
EEG = eeg_checkset( EEG );

% Highpass filter
EEG = pop_eegfiltnew(EEG, [], 1, [], true, [], 0); % Remove the numerical value to force recalculating every time
EEG.setname='hp1hz';
EEG = eeg_checkset( EEG );

% Lowpass filter
EEG = pop_eegfiltnew(EEG, [], 30, [], 0, [], 0); % Remove the numerical value to force recalculating every time
EEG.setname='lp30hz';
EEG = eeg_checkset( EEG );

% Epoch data to known markers and remove overlaps
EEG = pop_epoch( EEG, {  'face-inverted'  'face-upright'  'house-inverted'  'house-upright'  }, [-1  2], 'newname', 'segall', 'epochinfo', 'yes');
EEG = eeg_checkset( EEG );
EEG = pop_rmbase( EEG, [-200    0]);
EEG = eeg_checkset( EEG );

% Basic bad channel rejection
EEG = pop_rejchan(EEG, 'elec',[1:128] ,'threshold',3,'norm','on','measure','spec','freqrange',[1 30] );
EEG.setname='rmch';
EEG = pop_reref( EEG, []); % Rereference now that channels have been removed
EEG = eeg_checkset( EEG );

% Reject trials with extreme values
EEG = pop_eegthresh(EEG,1,[1:EEG.nbchan] ,-100,100,-1,1.998,2,0); % Modify to be dyanmic number of channels
EEG = eeg_rejsuperpose( EEG, 1, 1, 1, 1, 1, 1, 1, 1);
EEG = pop_rejepoch( EEG, find(EEG.reject.rejthresh) ,0); % Modify to read from structure as opposed to explicit numbering
EEG.setname='rmep';
EEG = eeg_checkset( EEG );

% Save resulting file
EEG = pop_saveset( EEG, 'filename','[batch_dfn,.,-1]_seg.set','filepath','[batch_dfp]');
EEG = eeg_checkset( EEG );
~~~
{: .language-matlab}
