### What You Want to Do:
Extend the subreport hyperlink functionality to open an editable version of the form. The default behavior opens a read-only form

### Code to Do It:
```javascript
var subreport = 'Caption for my subreport'

// callback executed when array is found
function handleDataArray(dataArray) {
  var subreport = Form.getFormLineByCaption('subreport');
  dataArray.
  forEach((row, index) => 
    document.
    getElementById(`sr-table-${subreport.formLinesId}-${subreport.subReportHeaderId}`).
    querySelectorAll("[id^='row']")[index].
    addEventListener("click", 
      function () {OpenFormGeneric( 
        "eventform.asp", //aspx file to use, defaults to Form.aspx
        "PROBLEMS_CLI", //form code 
        row.event_log_id, //keyValue, event_log_id for events
        "EDIT", //add, edit, or view mode
        true, //allow add? boolean
        true, //allow edit? boolean
        false, //allow delete? boolean
        false, 
        "{e64e464a-3e6e-43c7-9ab7-ba33980a7816}", // event definition id 
        parentValue, //parentValue
        serviceTrack, 
        null, 
        null, //program providing service
        null, //program enrollment id
        null, 
        false //does this complete a scheduled event? boolean
        );
      }
    )
  );
  // Give modified subreport a distinct style to indicate it is interactive.
  $(`[form_line_id="${Form.getFormLineByCaption('subreport').formLinesId}"]`)[0].style.fontStyle = "oblique";
  $(`[form_line_id="${Form.getFormLineByCaption('subreport').formLinesId}"]`)[0].style.color = "#26828EFF";
}

// set up the mutation observer
var observer = new MutationObserver(function (mutations, me) {
  // `mutations` is an array of mutations that occurred
  // `me` is the MutationObserver instance
  var dataArray = Form.getFormLineByCaption('subreport').srValue.rawDataSource;
  if (dataArray) {
    handleDataArray(dataArray);
    me.disconnect(); // stop observing
    return;
  }
});

// start observing
observer.observe(document, {
  childList: true,
  subtree: true
});
```
### Function Arguments
|Argument       |Definition |Data Type|
|---            |---        |---      |
|subreport |The caption for the subreport, as displayed on the form.|String|
|dataArray|The data values inside the subreport.|Array|

### Implementation Details
1. Define the subreport to display in the form.
  * For best results, follow Netsmart's instructions to create a hyperlinked subreport and confirm it successfully launches. 
  * Make sure the event_log_id is included in the subreport. It does not need to be visible.
2. Identify the form code for the form to launch.
3. Identify the event_definition_id for the event being edited.
4. Add the subreport to a form.
5. Adjust the contents of the `handleDataArray()` function as needed and paste it into the After Load box on the form containing the subreport.

### End-User Details
This functionality came out of a request for a streamlined process to end-date medications, diagnoses, strengths, and needs at the end of services. We were unable to find a way to link these all together using subforms, so links to forms were necessary.

Use of promises and MutationObserver were necessary because subreports signal that they are 'loaded' once the frame for the table is ready. The actual data contents arrive a little later. MutationObserver allows us to wait for the data to arrive and the table rows to populate before attempting to create hyperlinks.

Once implemented, open the form containing the subreport. If it is linked to an editable form, the subreport will render in blue-green italic type. This display can be customized as needed.

In a standard hyperlinked subreport, the link is in the Description Column for Link specified in Setup > User Tools > Sub Reports - User > User Sub Reports, rendered in blue, bold, underlined type.

![A hyperlinked subform](/JavaScript%20Functions/assets/images/Subreport%20loaded.png "Cursor hovering over the description column for link")

Clicking the linked column will bring up a read-only form corresponding to the cell clicked. Note the VIEW indicator in the top bar and the date fields are greyed out.

![A view-only form link](/JavaScript%20Functions/assets/images/Subreport%20View%20Only%20Link.png "A view-only form")

In the customized subreport, each full row is hyperlinked. Clicking on any other cell in a row will open the editable form.

![A hyperlinked subform](/JavaScript%20Functions/assets/images/Subreport%20loaded2.png "Cursor hovering over the actual date column in the subreport")

An editable form opens. Note the form is labeled as EDIT mode and the date fields are active.

![An editable form launched from a subreport](/JavaScript%20Functions/assets/images/Subreport%20Editable%20Link.png "An editable form launched from a subreport")


## Credit:
https://stackoverflow.com/questions/16149431/make-function-wait-until-element-exists

## To-Do 
Currently breaks when the subreport is refreshed. Will need to identify a way to re-apply hyperlinks after the refresh button is clicked.
