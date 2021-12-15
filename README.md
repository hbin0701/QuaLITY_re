# QuALITY: Question Answering with Long Input Texts, Yes!

**Authors**: Richard Yuanzhe Pang*, Alicia Parrish*, Nitish Joshi*, Nikita Nangia, Jason Phang, Angelica Chen, Vishakh Padmakumar, Johnny Ma, Jana Thompson, He He, and Samuel R. Bowman
(* = equal contribution)

## Data link

You can access the dataset [here](https://drive.google.com/drive/folders/1VjWjnD1SIVh3JmEEZ31M47svdEwttb5k?usp=sharing). 

## Paper preprint

You can read the paper [insert paper link]().

## Data README

Here are the explanations to the fields in the jsonl file. Each json line corresponds to the set of validated questions, corresponding to one article, written by one writer. 
- `article_id`: String. A five-digit number uniquely identifying the article. In each split, there are exactly two lines containing the same `article_id`, because two writers wrote questions for the same article.
- `set_unique_id`: String. The unique ID corresponding to the set of questions, which corresponds to the line of json. Each set of questions is written by the same writer.
- `batch_num`: String. The batch number. Our data collection is split in two groups, and there are three batches in each group. `[i][j]` means the j-th batch in the i-th group. For example, `23` corresponds to the third batch in the second group.
- `writer_id`: String. The anonymized ID of the writer who wrote this set of questions. 
- `source`: String. The source of the article. 
- `title`: String. The title of the article.
- `author`: String. The author of the article.
- `topic`: String. The topic of the article.
- `url`: String. The URL of the original unprocessed source article. 
- `article`: String. The HTML of the article. A script that converts HTML to plain texts is provided. 
- `questions`: A list of dictionaries explained below. Each line of json has a different number of questions because some questions were removed following validation.

As discussed, the value of `questions` is a list of dictionaries. Each dictionary has the following fields.
- `question`: The question. 
- `options`: A list of four answer options.
- `gold_label`: The correct answer, defined by a majority vote of 3 or 5 annotators + the original writer's label. The number corresponds to the option number (1-indexed) in `options`. 
- `writer_label`: The label the writer provided. The number corresponds to the option number (1-indexed) in `options`. 
- `validation`: A list of dictionaries containing the untimed validation results. Each dictionary contains the following fields.
    - `untimed_annotator_id`: The anonymized annotator IDs corresponding to the untimed validation results shown in `untimed_answer`.
    - `untimed_answer`: The responses in the untimed validation. Each question in the training set is annotated by three workers in most cases, and each question in the dev/test sets is annotated by five cases in most cases (see paper for exceptions). 
    - `untimed_eval1_answerability`: The responses (represented numerically) to the first eval question in untimed validation. We asked the raters: “Is the question answerable and unambiguous?” The values correspond to the following choices:
        - 1: Yes, there is a single answer choice that is the most correct.
        - 2: No, two or more answer choices are equally correct.
        - 3: No, it is unclear what the question is asking, or the question or answer choices are unrelated to the passage.
    - `untimed_eval2_context`: The responses (represented numerically) to the second eval question in untimed validation. We asked the raters: “How much of the passage/text is needed as context to answer this question correctly?” The values correspond to the following choices:
        - 1: Only a sentence or two of context.
        - 2: At least a long paragraph or two of context.
        - 3: At least a third of the passage for context.
        - 4: Most or all of the passage for context.
    - `untimed_eval3_distractor`: The responses to the third eval question in untimed validation. We asked the raters: “Which of the options that you did not select was the best "distractor" item (i.e., an answer choice that you might be tempted to select if you hadn't read the text very closely)?” The numbers correspond to the option numbers (1-indexed).
- `speed_validation`: A list of dictionaries containing the speed validation results. Each dictionary contains the following fields.
    - `speed_annotator_id`: The anonymized annotator IDs corresponding to the speed annotation results shown in `speed_answer`.
    - `speed_answer`: The responses in the speed validation. Each question is annotated by five workers.
- `difficult`: A binary value. `1` means that less than 50% of the speed annotations answer the question correctly, so we include this question in the `hard` subset. Otherwise, the value is `0`.

### Validation criteria for the questions
- More than 50% of annotators answer the question correctly in the untimed setting. That is, more than 50% of the `untimed_answer` annotations agree with `gold_label` (defined as the majority vote of validators' annotations together with the writer's provided label).
- More than 50% of annotators think that the question is unambiguous and answerable. That is, more than 50% of the `untimed_eval1_answerability` annotations have `1`'s.

### <a name="difficult">What are the `hard` questions?</a>
 - More than 50% of annotators answer the question correctly in the untimed setting. That is, more than 50% of the `untimed_answer` annotations agree with `gold_label`.
 - More than 50% of annotators think that the question is unambiguous and answerable. That is, more than 50% of the `untimed_eval1_answerability` annotations have `1`'s.
 - More than 50% of annotators answer the question incorrectly in the speed validaiton setting. That is, more than 50% of the `speed_answer` annotations are incorrect.

### Test set

The annotations for questions in the test set will not be released. The authors are currently working on a leaderboard. Stay tuned!

### Contact

{yzpang, alicia.v.parrish}@nyu.edu
