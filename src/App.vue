<script setup lang="ts">
import { ref, onMounted } from "vue";
import TheButton from "@/components/TheButton.vue";

const textField = ref<HTMLIFrameElement | null>(null);
const showCode = ref(false);

onMounted(() => {
  textField.value!.contentDocument!.designMode = "On";
});

const apply = (cmd: string) => {
  if (cmd === "showCode") {
    const textBody = textField.value?.contentDocument?.querySelector("body");

    if (showCode.value) {
      textBody!.innerHTML = textBody?.textContent!;
      showCode.value = false;
    } else {
      textBody!.textContent = textBody?.innerHTML!;
      showCode.value = true;
    }
  }
  if (cmd === "heading") {
    textField.value?.contentDocument?.execCommand("formatBlock", false, "<h1>");
  }
  if (cmd === "paragraph") {
    textField.value?.contentDocument?.execCommand("formatBlock", false, "<p>");
  }

  if (cmd === "copy") {
    const textBody = textField.value?.contentDocument?.querySelector("body");

    if (!showCode.value) {
      new Promise((resolve) => {
        textBody!.textContent = textBody?.innerHTML!;
        textField.value?.contentDocument?.execCommand("selectAll", false);
        textField.value?.contentDocument?.execCommand(cmd, false);

        resolve("done");
      }).then(() => {
        textBody!.innerHTML = textBody?.textContent!;
      });
    } else {
      textField.value?.contentDocument?.execCommand("selectAll", false);
      textField.value?.contentDocument?.execCommand(cmd, false);
    }
  }

  if (cmd === "insertImage" || cmd === "createLink") {
    let url = prompt("Enter link here: ", "");

    if (url) {
      textField.value?.contentDocument?.execCommand(cmd, false, url);
    }

    if (cmd === "insertImage") {
      const imgs = textField.value?.contentDocument?.querySelectorAll("img");
      imgs?.forEach((item) => {
        item.style.maxWidth = "500px";
      });
    } else {
      const links = textField.value?.contentDocument?.querySelectorAll("a");
      links?.forEach((item) => {
        item.target = "_blank";
        item.addEventListener("mouseover", () => {
          textField.value!.contentDocument!.designMode = "Off";
        });
        item.addEventListener("mouseout", () => {
          textField.value!.contentDocument!.designMode = "On";
        });
      });
    }
  } else {
    textField.value?.contentDocument?.execCommand(cmd as string, false);
  }
};
</script>

<template>
  <div class="container">
    <form>
      <div class="buttons">
        <TheButton
          color="#42b883"
          action="undo"
          @click="apply"
          view-box="0 0 512 512"
        />
        <TheButton
          color="#42b883"
          action="redo"
          @click="apply"
          view-box="0 0 512 512"
        />
        <TheButton
          color="#42b883"
          action="justifyLeft"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="justifyCenter"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="justifyRight"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="justifyFull"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="heading"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="paragraph"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="bold"
          @click="apply"
          view-box="0 0 384 512"
        />
        <TheButton
          color="#42b883"
          action="italic"
          @click="apply"
          view-box="0 0 384 512"
        />
        <TheButton
          color="#42b883"
          action="underline"
          @click="apply"
          view-box="0 0 448 512"
        />
        <TheButton
          color="#42b883"
          action="insertOrderedList"
          @click="apply"
          view-box="0 0 512 512"
        />
        <TheButton
          color="#42b883"
          action="insertUnorderedList"
          @click="apply"
          view-box="0 0 512 512"
        />
        <TheButton
          color="#42b883"
          action="insertImage"
          @click="apply"
          view-box="0 0 512 512"
        />
        <TheButton
          color="#42b883"
          action="createLink"
          @click="apply"
          view-box="0 0 640 512"
        />
        <TheButton
          color="#42b883"
          action="showCode"
          @click="apply"
          view-box="0 0 640 512"
          :active-border="true"
        />
        <TheButton
          color="#42b883"
          action="copy"
          @click="apply"
          view-box="0 0 448 512"
        />
      </div>
    </form>
    <iframe id="output" ref="textField" name="textField"></iframe>
  </div>
</template>

<style scoped></style>
