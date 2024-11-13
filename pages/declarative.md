# Declarative

## Installation

```terminal
npm i betahub-web-form
```

or

```terminal
yarn add betahub-web-form
```

```typescript
import BHWF, { transformIntoDropzone } from "betahub-web-form";
import "betahub-web-form/dist/bhwf.min.css";
import "betahub-web-form/dist/dropzone.min.css";
```

## Initialization

```typescript
type Args = {
  projectId?: string;
  apiKey?: string;
  formElement?: HTMLElement | HTMLFormElement;
  customElements?: {
    description?: {
      inputElement?: HTMLInputElement | HTMLTextAreaElement;
      errorMsgElement?: HTMLElement;
      validator?: (value: string) => [boolean, string | undefined];
    },
    stepsToReproduce?: {
      inputElement?: HTMLInputElement | HTMLTextAreaElement;
      errorMsgElement?: HTMLElement;
      validator?: (value: string) => [boolean, string | undefined];
    },
    media?: {
      inputElement?: HTMLInputElement;
      errorMsgElement?: HTMLElement;
      dropzone?: Dropzone;
      validator?: (value: File[]) => [boolean, string | undefined];
    },
    screenshots?: {
      inputElement?: HTMLInputElement;
      errorMsgElement?: HTMLElement;
      dropzone?: Dropzone;
      validator?: (value: File[]) => [boolean, string | undefined];
    },
    videos?: {
      inputElement?: HTMLInputElement;
      errorMsgElement?: HTMLElement;
      dropzone?: Dropzone;
      validator?: (value: File[]) => [boolean, string | undefined];
    },
    logs?: {
      inputElement?: HTMLInputElement;
      errorMsgElement?: HTMLElement;
      dropzone?: Dropzone;
      validator?: (value: File[]) => [boolean, string | undefined];
    }
  }
}
```

You need to manually get the form elements using `document.getElementById()` or similar function.

You can convert file input into dropzone using function:

```typescript
import { transformIntoDropzone } from "betahub-web-form";

const dropzone = transformIntoDropzone(mediaInputElement);
```

Example:

```typescript
import BHWF, { transformIntoDropzone } from "betahub-web-form";
import "betahub-web-form/dist/bhwf.min.css";
import "betahub-web-form/dist/dropzone.min.css";

const formElement = document.getElementById("form") as HTMLElement;
// ... other elements

const form = new BHWF.Form({
  projectId: "{PROJECT_ID}", // Set your project ID here
  apiKey: "{API_KEY}", // Set your API key here
  formElement,
  customElements: {
    description: {
      inputElement: descriptionInputElement,
      errorMsgElement: descriptionErrorMsgElement,
    },
    stepsToReproduce: {
      inputElement: stepsToReproduceInputElement,
      errorMsgElement: stepsToReproduceErrorMsgElement,
      validator: (value) => {
        if (!value) return [false, "Steps to reproduce are required"];
        return [true, undefined];
      },
    },
    media: {
      inputElement: mediaInputElement,
      errorMsgElement: mediaErrorMsgElement,
      dropzone: dropzone,
      validator: (value) => {
        if (value.length === 0) return [false, "Media are required"];
        return [true, undefined];
      },
    },
  },
});
```

## Events

```typescript
interface EventDataMap {
  loading: undefined;
  success: { issueId?: number, issueUrl?: string };
  inputError: { message?: string; input: Input | FileInput };
  apiError: { message?: string; status: number };
  progress: { progress?: number; message?: string };
  reset: undefined;
  cleanErrors: undefined;
};
```

Example:

```typescript
form.on("loading", () => {
  loadingModalElement.classList.add("bhwf-modal-show");
});
form.on("progress", (data) => {
  progressElement.innerText = data?.progress !== undefined ? `${Math.round(data.progress)}%` : "";
  progressMsgElement.innerText = data?.message || "";
});
form.on("apiError", (data) => {
  loadingModalElement.classList.remove("bhwf-modal-show");
  errorModalElement.classList.add("bhwf-modal-show");
  if (data?.status !== undefined) {
    errorTextElement.innerText = data?.message || "";
  }
});
form.on("inputError", () => {
  submitButtonElement.setAttribute("disabled", "true");
});
form.on("cleanErrors", () => {
  submitButtonElement.removeAttribute("disabled");
});
form.on("success", () => {
  loadingModalElement.classList.remove("bhwf-modal-show");
  successModalElement.classList.add("bhwf-modal-show");
});
```