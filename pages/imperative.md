# Imperative

The imperative approach lets you start using the library instantly, without any coding. Simply attach the script and set up a form with the specified structure in your HTML file.

# Quick Start

Just use the HTML, but in the form, set [your project ID and API Key](pages/faq?id=how-to-find-the-project-id-and-api-key). This is a similar form to the one presented as a demo, [here](/?id=demo).

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>BetaHub Web Form</title>

    <script src="https://unpkg.com/betahub-web-form@1.*.*/dist/bhwf.js"></script>
    <link
      rel="stylesheet"
      type="text/css"
      href="https://unpkg.com/betahub-web-form@1.*.*/dist/bhwf.min.css"
    />
    <link
      rel="stylesheet"
      type="text/css"
      href="https://unpkg.com/betahub-web-form@1.*.*/dist/dropzone.min.css"
    />

    <link
      href="https://fonts.googleapis.com/css2?family=Assistant:wght@200..800&display=swap"
      rel="stylesheet"
    />
    <style>
      body {
        box-sizing: border-box;
        min-height: calc(100vh - 40px);
        margin: 20px;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
        flex-wrap: wrap;
        gap: 20px;

        font-family: "Assistant", sans-serif;
      }
      body > * {
        box-sizing: border-box;
        width: 100%;
        max-width: 473px;
        height: fit-content;
      }
      input,
      textarea {
        font-family: "Assistant", sans-serif;
      }
      p {
        margin: 0;
      }
      [data-bhwf-progress] {
        font-size: 22px;
      }
    </style>
  </head>
  <body>
    <form
      data-bhwf-form
      data-bhwf-project-id="{YOUR_PROJECT_ID}"
      data-bhwf-api-key="{YOUR_API_KEY}"
    >
      <h1>Submit your issue</h1>

      <label>Description</label>
      <div class="bhwf-input-container">
        <textarea
          data-bhwf-input="description"
          data-bhwf-counter
          placeholder="Describe your issue as detailed as possible"
        ></textarea>
      </div>
      <span data-bhwf-error-msg="description"></span>

      <label>Steps to Reproduce</label>
      <textarea
        data-bhwf-input="stepsToReproduce"
        placeholder="Describe the steps leading to the issue"
      ></textarea>

      <label>Attach Media</label>
      <input type="file" data-bhwf-input="media" data-bhwf-dropzone multiple />

      <button data-bhwf-button="submit">Submit</button>

      <div data-bhwf-modal="loading">
        <div class="bhwf-loader"></div>
        <br />
        <span data-bhwf-progress></span>
        <span data-bhwf-progress-msg></span>
      </div>

      <div data-bhwf-modal="error">
        <h2>Something went wrong!</h2>
        <p>Your issue couldn't be sent right now</p>
        <span data-bhwf-error-msg="api"></span>
        <br />
        <button data-bhwf-button="tryAgain">Try again</button>
        <button data-bhwf-button="reset">Reset</button>
      </div>

      <div data-bhwf-modal="success">
        <h2>All done!</h2>
        <p>Thank you for submitting your issue</p>
        <br />
        <a data-bhwf-button="viewIssue" href="" target="_blank">
          View Your Issue
        </a>
        <button data-bhwf-button="reset">Submit another issue</button>
      </div>
    </form>
  </body>
</html>
```

# Installation

The library is deployed on the [npm](https://www.npmjs.com/package/betahub-web-form) and in the HTML file, you can attach it using service such as [UNPKG](https://unpkg.com/).

```html
  <head>
    <script src="https://unpkg.com/betahub-web-form@1.*.*/dist/bhwf.js"></script>
  </head>
```

We also recommend to use the CSS files with default styling

```html
  <head>
    <link
      rel="stylesheet"
      type="text/css"
      href="https://unpkg.com/betahub-web-form@1.*.*/dist/bhwf.min.css"
    />
    <link
      rel="stylesheet"
      type="text/css"
      href="https://unpkg.com/betahub-web-form@1.*.*/dist/dropzone.min.css"
    />
  </head>
```

> [!INFO]
> If you don't trust the CDN, you can just download the `bhwf.js`, `bhwf.min.css` and `dropzone.min.css` from the dist folder of [the library repo](https://github.com/patchkit-net/betahub-web-form) and include them with your sources.

# Form elements

Every element can contain standard html attributes and custom styling.

## Form

The outer element creating a context for its content.

* **Required**
* **Allowed HTML tags:** form, div
* **Attribute:** `data-bhwf-form`
* **Settings attributes:**
  * `data-bhwf-project-id`="{PROJECT_ID}" - (required) target project ID
  * `data-bhwf-api-key`="{API_KEY}" - (required) project auth token
* **Settings classes:**
  * `light` - (optional) set the color theme to light
* **State attributes:**
  * `data-bhwf-state`="inputError" - when some inputs didn't pass the validation
  * `data-bhwf-state`="loading" - when form is submitting
  * `data-bhwf-state`="apiError" - when there was an error with a request
  * `data-bhwf-state`="success" - when the form was successfully submitted
* **State classes:**
  * `bhwf-error` - when any error occurs

Example configuration:

```html
<form
  data-bhwf-form
  data-bhwf-project-id="example-project-id"
  data-bhwf-api-key="example-api-key"
  class="light"
>
  <!-- Form content -->
</form>
```

Result during work:

```html
<form
  data-bhwf-form
  data-bhwf-project-id="example-project-id"
  data-bhwf-api-key="example-api-key"
  class="light bhwf-error"
  data-bhwf-state="apiError"
>
  <!-- Form content -->
</form>
```

## Input

* **Allowed HTML tags:** textarea, input[type="text"]
* **Attribute:** `data-bhwf-input`="{INPUT_NAME}"
* **Settings attributes:**
  * `data-bhwf-counter` - appends to the input parent element a character counter for the field
* **State classes:**
  * `bhwf-error` - when validation didn't pass

### Description Input

The value of this input is analyzed by BetaHub AI to generate issue title, priority, tags, etc.

* **Required**
* **Attribute:** `data-bhwf-input`="description"
* **Default validation:**
  * No empty
  * More than 50 charaters

Example configuration:

```html
<textarea
  data-bhwf-input="description"
></textarea>
```

If you want to use the counter, you need a container with `position: relative` style

```html
<div class="bhwf-input-container">
  <textarea
    data-bhwf-input="description"
    data-bhwf-counter
    placeholder="Describe your issue as detailed as possible"
  ></textarea>
</div>
```

Result during work:

```html
<div class="bhwf-input-container">
  <textarea
    data-bhwf-input="description"
    data-bhwf-counter
    placeholder="Describe your issue as detailed as possible"
    class="bhwf-error"
  ></textarea>
  <span class="char-counter">123</span>
</div>
```

### Steps to Reproduce Input

The value of this input is analyzed by BetaHub AI to determine the list of steps, so the format is not strict - it can be a comma-separated list, a story or anything else.

* **Attribute:** `data-bhwf-input`="stepsToReproduce"

Example configuration:

```html
<textarea
  data-bhwf-input="stepsToReproduce"
></textarea>
```

## File Input

* **Allowed HTML tags:** input[type="file"]
* **Attribute:** `data-bhwf-input`="{FILE_INPUT_NAME}"
* **Settings attributes:**
  * `data-bhwf-dropzone` - changes standard file input into a dropzone unsing [Dropzone.js](https://www.dropzone.dev/) (hiding the input, and adding the dropzone next to it)
  * `multiple` - used to allow multiple files, used to configure dropzone
  * `accept`="{FILE_TYPES}" - used to specify file types, used to configure dropzone
* **State classes:**
  * `bhwf-error` - when validation didn't pass

### Media File Input

This is universal file input. The uploaded attachments are automatically sorted between screenshots, videos and log files.

* **Attribute:** `data-bhwf-input`="media"

Example configuration:

```html
<input 
  type="file"
  data-bhwf-input="media"
  data-bhwf-dropzone
  multiple
/>
```

Result during work:

```html
<input 
  type="file"
  data-bhwf-input="media"
  data-bhwf-dropzone
  multiple
  class="bhwf-error"
  style="display: none;"
/>
<form class="dropzone dz-clickable bhwf-error">
  <div class="dz-default dz-message">
    <button class="dz-button" type="button">Drop files here to upload<button>
  </div>
</form>
```

### Screenshots File Input

* **Attribute:** `data-bhwf-input`="screenshots"
* **Default validation:**
  * Only image files

Example configuration:

```html
<input 
  type="file"
  data-bhwf-input="screenshots"
  data-bhwf-dropzone
  multiple
  accept="image/*"
/>
```

Result during work:

```html
<input 
  type="file"
  data-bhwf-input="screenshots"
  data-bhwf-dropzone
  multiple
  accept="image/*"
  class="bhwf-error"
  style="display: none;"
/>
<form class="dropzone dz-clickable bhwf-error">
  <div class="dz-default dz-message">
    <button class="dz-button" type="button">Drop files here to upload<button>
  </div>
</form>
```

### Videos File Input

* **Attribute:** `data-bhwf-input`="videos"
* **Default validation:**
  * Only video files

Example configuration:

```html
<input 
  type="file"
  data-bhwf-input="videos"
  data-bhwf-dropzone
  multiple
  accept="video/*"
/>
```

Result during work:

```html
<input 
  type="file"
  data-bhwf-input="videos"
  data-bhwf-dropzone
  multiple
  accept="video/*"
  class="bhwf-error"
  style="display: none;"
/>
<form class="dropzone dz-clickable bhwf-error">
  <div class="dz-default dz-message">
    <button class="dz-button" type="button">Drop files here to upload<button>
  </div>
</form>
```

### Logs File Input

* **Attribute:** `data-bhwf-input`="logs"
* **Default validation:**
  * All files except images and videos

Example configuration:

```html
<input 
  type="file"
  data-bhwf-input="logs"
  data-bhwf-dropzone
  multiple
/>
```

Result during work:

```html
<input 
  type="file"
  data-bhwf-input="logs"
  data-bhwf-dropzone
  multiple
  class="bhwf-error"
  style="display: none;"
/>
<form class="dropzone dz-clickable bhwf-error">
  <div class="dz-default dz-message">
    <button class="dz-button" type="button">Drop files here to upload<button>
  </div>
</form>
```

## Error Message

* **Allowed HTML tags:** div, span
* **Attribute:** `data-bhwf-error-msg`="{FILE_INPUT_NAME}"
* **Variable content:** - inner text displays error message

Example usage:

```html
<span data-bhwf-error-msg="description"></span>
```

Result during work:
```html
<span data-bhwf-error-msg="description">Description is required</span>
```

There is a special element for the API errors not connected with any input:

```html
<span data-bhwf-error-msg="api"></span>
```

## Progress

* **Allowed HTML tags:** div, span
* **Attribute:** `data-bhwf-progress`
* **Variable content:** - inner text displays issue creating and file uploading progress percentage

Example configuration:

```html
<span data-bhwf-progress></span>
```

Result during work:

```html
<span data-bhwf-progress>95%</span>
```

## Progress Message

* **Allowed HTML tags:** div, span
* **Attribute:** `data-bhwf-progress-msg`
* **Variable content:** - inner text displays current task

Example configuration:

```html
<span data-bhwf-progress-msg></span>
```

Result during work:

```html
<span data-bhwf-progress-msg>Uploading files: 4/5</span>
```

## Buttons

* **Allowed HTML tags:** div, span, a, button
* **Attribute:** `data-bhwf-button`="{BUTTON_TYPE}"

### Submit Button

Validates and submits the form.

Example configuration:

```html
<button data-bhwf-button="submit">Submit</button>
```

### Reset Button

Resets all inputs and clears errors.

Example configuration:

```html
<button data-bhwf-button="reset">Reset</button>
```

### Try Again Button

Closes error modal, to allow for sending the form again.

Example configuration:

```html
<button data-bhwf-button="tryAgain">Try Again</button>
```

### View Issue Button

Opens the created issue.

* **Allowed HTML tags:** a

Example configuration:

```html
<a 
  data-bhwf-button="viewIssue"
  href=""
  target="_blank"
>
  View Your Issue
</a>
```

## Modal

Modals will be automatically displayed and hidden depending on the state of the form.

* **Allowed HTML tags:** div
* **Attribute:** `data-bhwf-modal`="{MODAL_TYPE}"

### Loading Modal

* **Attribute:** `data-bhwf-modal`="loading"

Example configuration:

```html
<div data-bhwf-modal="loading">
  <div class="bhwf-loader"></div>
  <br />
  <span data-bhwf-progress></span>
  <span data-bhwf-progress-msg></span>
</div>
```

### Error Modal

* **Attribute:** `data-bhwf-modal`="error"

Example configuration:

```html
<div data-bhwf-modal="error">
  <h2>Something went wrong!</h2>
  <p>Your issue couldn't be sent right now</p>
  <span data-bhwf-error-msg="api"></span>
  <br />
  <button data-bhwf-button="tryAgain">Try again</button>
  <button data-bhwf-button="reset">Reset</button>
</div>
```

### Success Modal

* **Attribute:** `data-bhwf-modal`="success"

Example configuration:

```html
<div data-bhwf-modal="success">
  <h2>All done!</h2>
  <p>Thank you for submitting your issue</p>
  <br />
  <a data-bhwf-button="viewIssue" href="" target="_blank">
    View Your Issue
  </a>
  <button data-bhwf-button="reset">Submit another issue</button>
</div>
```