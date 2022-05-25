### What You Want to Do:
Extend the capabilities of getTestAnswerValue for multiple-select (checklist) questions and set test answers from JavaScript

### Code to Do It:
```javascript
function getTestSelections(test_code, question_code, returnCaption = true) {
  return Form.
    getTestFormLineByCode(test_code, Form.formObject).
    testValue.
    Questions.
    filter(x => x.output_code == question_code && x.details_type_code.slice(-4) == "LIST")[0].
    Answers.
    filter(x => x.value).
    map(x => x[returnCaption ? "test_setup_answers_caption" : "test_setup_answers_value"])
}

function setTestAnswerValue(test_code, question_code, answer_caption, newValue) {
  let question = Form.
    getTestFormLineByCode(test_code).
    testValue.
    Questions.
    filter(q => q.output_code == question_code)[0];

  let answer = question.
    Answers.
    filter(a => a.test_setup_answers_caption == answer_caption)[0];

  if (question.details_type_code.slice(-4 == 'LIST')) {
    if (answer.value ^ newValue) {
      question.
      getField().
      querySelectorAll(`[test_setup_details_answers_id="${answer.test_setup_details_answers_id}"]`)[0].
      click();}
  } else {
    question.
    getField.
    value = newValue;
  }
}
```

### Function Arguments
|Argument       |Definition |Data Type|
|---            |---        |---      |
|test_code|The shortcut code for the test. Specified in Test Design.|String|
|question_code|The shortcut code for the specific test item.|String|
|returnCaption|Toggle whether to return the answer caption or the numeric answer value. |Boolean. Defaults to `true`|
|answer_caption|The caption of the response option to update.|String|
|newValue|The value to set the test answer to.|String or Boolean. Single- and multiple-select items take a Boolean value.|

### Implementation Details
These functions are modeled after the `getTestAnswerValue` function, and have the same requirements for test and question codes.

`getTestSelections` will work for both single- and multiple-select items, but the answer captions do not need to be specified in advance. The function will return an array of selections, or an empty array if none are selected.

### End-User Details
