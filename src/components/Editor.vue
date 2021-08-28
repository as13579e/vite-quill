<template>
  <h1>{{ msg }}</h1>
  <div id="editor"></div>
</template>
<script setup lang="ts">
import Quill from "quill";
import Mention from "quill-mention";
import { onMounted } from "vue";
Quill.register("modules/mention", Mention);
const msg = "quill";
const atValues = [
  { id: "515fd775-cb54-41f3-b921-56163871e2cf", value: "Mickey Dooley " },
  {
    id: "3f0b7933-57b8-4d9d-b238-f8af62b2e945",
    value: "Desmond Waterstone",
  },
  { id: "711f68ab-ca20-4011-ab0f-d98c8fac4c05", value: "Jeralee Fryd " },
  { id: "775e05fc-72bc-48a1-9508-5c61674734f1", value: "Eddie Hucquart " },
  { id: "e8701885-105e-4a21-b200-98e559776655", value: "Nathalia Whear " },
];

const hashValues = [
  { id: "0075256a-19c2-4a2d-b549-627000bcc3bc", value: "Accounting " },
  {
    id: "91e8901b-e3bf-4158-8ddf-7f5d9e8cbb7f",
    value: "Product Management",
  },
  { id: "c3373e89-7ab8-4a45-8b69-0b0cc49d89a9", value: "Marketing " },
  { id: "fa22f1d2-16c8-4bea-b869-8acad16e187a", value: "Engineering " },
  { id: "fe681168-f315-42f0-b78b-b1ea787fa1fd", value: "Accounting " },
];

onMounted(() => {
  new Quill("#editor", {
    placeholder: "Start by typing @ for mentions or # for hashtags...",
    modules: {
      mention: {
        allowedChars: /^[A-Za-z\sÅÄÖåäö]*$/,
        mentionDenotationChars: ["@"],
        spaceAfterInsert:false,
        source: function (searchTerm: any, renderList: any, mentionChar: any) {
          let values: any;

          if (mentionChar === "@") {
            values = atValues;
          } else {
            values = hashValues;
          }

          if (searchTerm.length === 0) {
            renderList(values, searchTerm);
          } else {
            const matches = [];
            for (let i = 0; i < values.length; i++)
              if (
                ~values[i].value.toLowerCase().indexOf(searchTerm.toLowerCase())
              )
                matches.push(values[i]);
            renderList(matches, searchTerm);
          }
        },
      },
    },
  });
});
</script>

<style >
#editor {
  height: 300px;
  margin: 30px;
}
body {
  font-size: 16px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  max-width: 800px;
  margin: 0 auto;
}

.ql-editor {
  border: 1px solid #a3a3a3;
  border-radius: 6px;
}

.ql-editor-disabled {
  border-radius: 6px;
  background-color: rgba(124, 0, 0, 0.2);
  transition-duration: 0.5s;
}

.ql-editor:focus {
  border: 1px solid #025fae;
}
</style>
