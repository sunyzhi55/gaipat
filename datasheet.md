# Datasheet for the GAIPAT dataset

Questions from the [Datasheets for Datasets](https://arxiv.org/abs/1803.09010) paper, v7.

Jump to section:

- [Motivation](#motivation)
- [Composition](#composition)
- [Collection process](#collection-process)
- [Preprocessing/cleaning/labeling](#preprocessingcleaninglabeling)
- [Uses](#uses)
- [Distribution](#distribution)
- [Maintenance](#maintenance)

## Motivation

### For what purpose was the dataset created?

This dataset was built to study the ocular behavior of human operators during assembly tasks, with the aim of predicting future actions of a human operator in the context of human-cobot collaboration.

### Who created the dataset (e.g., which team, research group) and on behalf of which entity (e.g., company, institution, organization)?

The dataset was created by researchers from the [Laboratoire d’Informatique de Grenoble](https://www.liglab.fr/en).

### Who funded the creation of the dataset?

This work  was partially funded by the LIG Emergence Eyes-Of-Cobots project.

## Composition

### What do the instances that comprise the dataset represent (e.g., documents, photos, people, countries)?

Each instance represents a participant’s ocular data (gaze points, pupil diameter, and confidence values) captured during a LEGO assembly task, enriched with:

* World state information: Current configuration of the LEGO assembly.
* Task instructions: The instructions provided for the assembly step.
* Human actions: Participant actions linked with ocular data (e.g., grasp a LEGO piece, release a LEGO).

### How many instances are there in total (of each type, if appropriate)?

The dataset includes 80 participants and 6 recorded steps of LEGO assembly.

### Does the dataset contain all possible instances or is it a sample (not necessarily random) of instances from a larger set?

This dataset is a sample of ocular data and task actions collected under controlled conditions. It is not exhaustive but is designed to represent diverse participant behaviors during assembly.

### What data does each instance consist of?

The file structure is as follows:

```
├── setup
│   ├── blocks.csv
│   ├── participants.csv
│   ├── glasses.csv
│   ├── instructions_{car,tb,house,tc,sc,tsb}.csv
│   └── slides_{car,tb,house,tc,sc,tsb}.csv
│              
├── participants
│   └── participant_id_XXX
│         ├── table
│         │   ├── gazepoints.csv
│         │   ├── states.csv
│         │   └── pupil_infos.zip
│         └── screen
│             ├── gazepoints.csv
│             ├── states.csv
│             └── pupil_infos.zip
├── README.md
```

The dataset designed for this study comprises data collected before and after the assembly of figures by participants. It is structured into two main categories: general data and assembly-specific data.

**Setup Data**

The setup data are located in the `setup` directory. It includes information collected before the figure assembly. The main files are:

- `blocks.csv`: Contains descriptions of the blocks.
- `instructions_{figure_name}.csv`: Provides instructions for assembling each figure.
- `slides_{figure_name}.csv`: Describes the slides used during the figure assembly.
- `participants.csv`: Contains descriptions of the participants.
- `glasses.csv`: Lists the distribution of glasses and contact lenses worn by participants.

**Participants Data**

The participants data are located in the `participants/[participant_id]` directory. It is subdivided into two subdirectories for each figure:

- `participants/participant_id/figure_name/table`
  - `gazepoints.csv`: Gazepoints on the table.
  - `events.csv`: Events on the table.
  - `states.csv`: Table state.
  - `pupil_info.csv`: Pupil size and data confidence reported by the eye tracker.

- `participants/participant_id/figure_name/screen`
  - `gazepoints.csv`: Gazepoints on the screen.
  - `events.csv`: Events on the screen (Buttons).
  - `states.csv`: Screen state.
  - `pupil_info.csv`: Pupil size and data confidence reported by the eye tracker.

### Is there a label or target associated with each instance?

Each instance is labeled with the participant ID and eye tracking configuration.

### Is any information missing from individual instances?

Instances may occasionally lack complete ocular data due to tracking inaccuracies or calibration issues, but the world state and human action information are consistently recorded. Also, some recording was impossible. Data missing are reported in the `setup/participants.csv` file

### Are relationships between individual instances made explicit (e.g., users’ movie ratings, social network links)?

Yes, relationships are explicit through:

* **Temporal alignment**: Ocular data is synchronized with world state and participant actions.
* **Step-based organization**: Each instruction step and its corresponding world state are linked to the participant’s ocular and action data.

### Are there recommended data splits (e.g., training, development/validation, testing)?

No.

### Are there any errors, sources of noise, or redundancies in the dataset?

Potential noise sources include:

* **Eye-tracking inaccuracies**: Loss of gaze data due to blinks or equipment misalignment.
* **Participant variability**: Differences in task execution strategies or environmental distractions.

### Is the dataset self-contained, or does it link to or otherwise rely on external resources (e.g., websites, tweets, other datasets)?

The dataset is self-contained and does not rely on external resources.

### Does the dataset contain data that might be considered confidential (e.g., data that is protected by legal privilege or by doctor-patient confidentiality, data that includes the content of individuals’ non-public communications)?

No, the dataset only includes anonymized information.

### Does the dataset contain data that, if viewed directly, might be offensive, insulting, threatening, or might otherwise cause anxiety?

No, the dataset does not contain any such data.

### Does the dataset relate to people?

Yes, it includes data from participants performing assembly tasks.

### Does the dataset identify any subpopulations (e.g., by age, gender)?

No. No demographic data (e.g. gender, age) have been included in the dataset.

### Is it possible to identify individuals (i.e., one or more natural persons), either directly or indirectly (i.e., in combination with other data) from the dataset?

No, the dataset is fully anonymized, and participant identities cannot be reconstructed.

### Does the dataset contain data that might be considered sensitive in any way (e.g., data that reveals racial or ethnic origins, sexual orientations, religious beliefs, political opinions or union memberships, or locations; financial or health data; biometric or genetic data; forms of government identification, such as social security numbers; criminal history)?

No, the dataset does not contain any sensitive data.

## Collection process

### How was the data associated with each instance acquired?

The data associated with each instance was directly observable and recorded during a controlled experiment. Specifically:

* **Ocular data** (gaze points, pupil diameter, confidence) was captured using eye-tracking devices.
* **Human actions** were tracked using synchronized recordings and annotations.

All data was validated through synchronization checks between the ocular data, logged actions, and task instructions to ensure consistency.

### What mechanisms or procedures were used to collect the data (e.g., hardware apparatus or sensor, manual human curation, software program, software API)?

Participants performed assembly tasks in both sitting and standing positions to cover a broad range of industrial scenarios. The table height was adjusted for each participant in accordance with the European ergonomic standard ISO-14738.

Participants were unfamiliar with the specific assemblies they needed to complete, which were communicated to them through an instruction screen. This screen, positioned next to the table, is operated via buttons located underneath to avoid disrupting the assembly process.

Two different eye-tracking configurations were used in the dataset to record human gaze:

1. ***Remote configuration:*** In this setup, devices are positioned at various locations within the participant’s workspace. We used the [Fovio FX3](https://www.eyetracking.com/fx3-remote-eye-tracking/) and the [Tobii 4C](https://help.tobii.com/hc/en-us/sections/360001811457-Tobii-Eye-Tracker-4C). The Fovio FX3 tracked eye movements on the table. We selected this remote eye-tracker for its ability to monitor horizontal areas, unlike most devices designed for vertical screens. According to Fovio’s documentation, it is positioned at the bottom of the table to avoid eyebrow occlusion and ensure effective calibration. However, user arm movements may obstruct tracking. Data from the Fovio FX3 were recorded using [Fovio’s Eyeworks software](https://www.eyetracking.com/eyeworks-software/). The Tobii 4C --upgraded with an 'analytical use' license-- tracked eye movements on the instruction screen. Initially designed for gaming, the Tobii 4C is less accurate than other Tobii models but is more tolerant of head movement and supports a greater distance between the tracker and participants. Tobii 4C data were collected using custom Python scripts.
2. ***Head-mounted configuration:*** In this setup, participants wore [Pupil Labs Core eye-tracking glasses](https://pupil-labs.com/products/core), equipped with a scene camera above the right eye and two infrared cameras to measure eye movement. We used [AprilTag](https://github.com/AprilRobotics/apriltag-imgs) markers from the 'tag36h11' family to map the assembly workspace to the scene camera. The [Pupil Labs Capture software](https://docs.pupil-labs.com/core/software/pupil-capture/) was used for scene mapping, device calibration, and data collection.

### Who was involved in the data collection process (e.g., students, crowdworkers, contractors) and how were they compensated (e.g., how much were crowdworkers paid)?

We recruited 80 psychology students who volunteered to participate in the study, divided into four groups of 20 based on the remote and head-mounted configurations, with participants either standing or sitting. No personal data (e.g., gender, age, health) was collected. Participants were compensated with experience points for their semester exams. The inclusion criteria required participants to be able to read and understand instructions in French and physically move construction blocks. Since the tasks could be performed while either sitting or standing, the ability to stand was not a criterion for inclusion.

### Over what timeframe was the data collected?

The data were collected during the 2023/24 winter.

### Were any ethical review processes conducted (e.g., by an institutional review board)?

This experimental protocol was submitted to and validated by CERGA, the Grenoble Alpes University ethics committee, and received the favorable opinion CERGA-Avis-2023-32.


### Does the dataset relate to people?

Yes, it includes data from participants performing assembly tasks.

### Did you collect the data from the individuals in question directly, or obtain it via third parties or other sources (e.g., websites)?

No.

### Were the individuals in question notified about the data collection?

No.

### Did the individuals in question consent to the collection and use of their data?

Yes.

### If consent was obtained, were the consenting individuals provided with a mechanism to revoke their consent in the future or for certain uses?

Participants have retained their unique identifiers and can request the deletion of their data.

### Has an analysis of the potential impact of the dataset and its use on data subjects (e.g., a data protection impact analysis) been conducted?

Yes. this was discuss was and accepted by the ethical committee.

## Preprocessing/cleaning/labeling


### Was any preprocessing/cleaning/labeling of the data done (e.g., discretization or bucketing, tokenization, part-of-speech tagging, SIFT feature extraction, removal of instances, processing of missing values)?

Yes. Raw data have been standardized. Raw data collected and python script sed are linked in the README.

### Was the “raw” data saved in addition to the preprocessed/cleaned/labeled data (e.g., to support unanticipated future uses)?

Yes. Raw data are [publicaly available](https://gricad-gitlab.univ-grenoble-alpes.fr/eyesofcobot/gaipat_rawdata).

### Is the software used to preprocess/clean/label the instances availab# Datasheet for the GAIPAT dataset

[Custom python scripts](https://gricad-gitlab.univ-grenoble-alpes.fr/eyesofcobot/gaipat_builder) have been used.

## Uses

### Has the dataset been used for any tasks already?

The dataset has already been used to predict human intentions. This paper is currently being submitted to a conference.

### Is there a repository that links to any or all papers or systems that use the dataset?

No.

### Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses?

Not to the best of our knowledge.

### Are there tasks for which the dataset should not be used?

Not to the best of our knowledge.

## Distribution

### Will the dataset be distributed to third parties outside of the entity (e.g., company, institution, organization) on behalf of which the dataset was created?

The dataset is open source and publicaly available.

### How will the dataset will be distributed (e.g., tarball on website, API, GitHub)?

The dataset is open source and publicaly available on [gitlab](The dataset is open source and publicaly available.).


### Will the dataset be distributed under a copyright or other intellectual property (IP) license, and/or under applicable terms of use (ToU)?

The dataset is distributed under the LGPL licence. Please refer to the [licence file](LICENCE.md).

### Have any third parties imposed IP-based or other restrictions on the data associated with the instances?

No restriction.

## Maintenance

_These questions are intended to encourage dataset creators to plan for dataset maintenance
and communicate this plan with dataset consumers._

### Who is supporting/hosting/maintaining the dataset?

The dataset is hosted and maintained on the GitLab instance of the University Grenoble Alpes, ensuring long-term accessibility and secure hosting. Ongoing maintenance and support are provided by the permanent researchers of the Laboratoire d'Informatique de Grenoble.

### How can the owner/curator/manager of the dataset be contacted (e.g., email address)?

For questions, please contact:
- Maxence Grand
- Maxence.Grand@univ-grenoble-alpes.fr
- Université Grenoble Alpes, Laboratoire d'Informatique de Grenoble, Teams Marvin/M-PSI


### Is there an erratum?

Currently, there are no erratum for this initial version. If any errors are identified, they will be documented in the CHANGELOG, which will be regularly updated and accessible through the README

### Will the dataset be updated (e.g., to correct labeling errors, add new instances, delete instances)?

Yes, the dataset will be updated periodically. Each update will be documented in the CHANGELOG, and the README will include a direct link to allow users to track changes

### If the dataset relates to people, are there applicable limits on the retention of the data associated with the instances (e.g., were individuals in question told that their data would be retained for a fixed period of time and then deleted)?

Participants have retained their unique identifiers and can request the deletion of their data. If such a request is made, the dataset will be updated accordingly, and the change will be documented in the CHANGELOG.

### Will older versions of the dataset continue to be supported/hosted/maintained?

Since this is the initial release, there are no previous versions to support. Future updates will be documented in the CHANGELOG, and the README will inform users of any changes.

### If others want to extend/augment/build on/contribute to the dataset, is there a mechanism for them to do so?

Yes, contributions are welcome. There are two types of contributions:

1. Adding new data using the existing dataset, such as for fixation detection. Contributors will need to provide access to the code used to generate the new data.
2. Adding new data through new experiments, which requires exact replication of the original experimental setup and full anonymization of participant data.

Each contribution will be documented in the CHANGELOG, and contributors will be cited. To propose a contribution, please follow the [GitLab contribution workflow](https://docs.gitlab.com/ee/development/contributing/first_contribution/). For questions, please contact the authors via the email addresses listed in the README.
