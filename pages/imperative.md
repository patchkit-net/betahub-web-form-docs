# Imperative

The imperative approach allows you to use the library quickly, without programming. All you need to do is attach a script and create a form with the appropriate structure in HTML.

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
        <a data-bhwf-button="viewIssue" href="" target="_blank"
          >View Your Issue</a
        >
        <button data-bhwf-button="reset">Submit another issue</button>
      </div>
    </form>
  </body>
</html>
```

# Installation

