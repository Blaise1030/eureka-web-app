<template>
  <div class="frame">
    <h2>The Incubator</h2>
    <Loader v-if="isLoading" />

    <div class="workspace-body" v-else>
      <div class="empty-state" v-if="isEmpty">
        <img class="empty-image" src="@/assets/not-found-icon.svg" />
        <p>Seems like you don't have any workspace now</p>
        <p>Create One !</p>
      </div>

      <div class="workspace-list" v-if="!isEmpty && showList">
        <div class="workspace-card">This is the workspace list</div>
      </div>

      <div class="student-join" v-if="showStudentJoin">
        <InputField
          :id="'workspace-code'"
          :label="'Workspace Code'"
          :type="'text'"
        />
        <Button class="button" :text="'Join'" />
      </div>

      <div class="settings-page" v-if="showSettingsPage">
        <InputField
          class="input"
          :key="index"
          v-for="(field, index) in lecturerSettings"
          :id="`${index} ${field.title}`"
          :label="field.title"
          :type="field.type"
        />
      </div>

      <button class="floating-action-button" v-on:click="onAddWorkspaceClick">
        <p v-if="showList">+</p>
        <p v-else><span class="material-icons"> arrow_back </span></p>
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { onMounted, ref } from "vue";
import Loader from "../common/Loader.vue";
import InputField from "../common/InputField.vue";
import Button from "../common/Button.vue";

// type InputSettings = {
//   text: string;
//   type: string;
// };
export default {
  name: "Incubator",
  components: { Loader, InputField, Button },
  data() {
    return {
      showList: true,
      isEmpty: false,
      showStudentJoin: false,
      showSettingsPage: false,
    };
  },
  methods: {
    onAddWorkspaceClick() {
      this.canCreateRooms
        ? (this.showSettingsPage = !this.showSettingsPage)
        : (this.showStudentJoin = !this.showStudentJoin);
      this.showList = !this.showList;
    },
  },
  setup() {
    var isLoading = ref(true);
    var canCreateRooms = ref(true);
    var lecturerSettings = ref([
      {
        title: "Workspace name",
        type: "text",
      },
      {
        title: "Max Members Per Team",
        type: "number",
      },
      {
        title: "Max Number Of Teams",
        type: "number",
      },
      {
        title: "Team Creation Deadline",
        type: "date",
      },
      {
        title: "Team Adjourning Date",
        type: "date",
      },
    ]);
    onMounted(() => {
      setTimeout(() => {
        isLoading.value = false;
      }, 2000);
    });
    return {
      isLoading,
      lecturerSettings,
      canCreateRooms,
    };
  },
};
</script>

<style lang="scss" scoped>
.frame {
  max-width: 60vw;
  margin: auto;
  display: flex;
  padding: 10px 15px;
  flex-direction: column;
  align-items: center;

  @include for-mobile {
    max-width: 100vw;
  }

  .empty-state {
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    margin-top: 60px;
    display: flex;
    height: 100%;

    p {
      @include for-mobile {
        font-size: 14px;
      }
    }

    .empty-image {
      margin-bottom: 20px;
      height: 100px;
    }
  }

  .student-join {
    margin: 50px 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .button {
      margin-top: 10px;
    }
  }
  .settings-page {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    .input {
      width: 20rem;
      margin-top: 1rem;
    }
  }
  .floating-action-button {
    @include shadow;
    margin: 50px;
    position: absolute;
    right: 0;
    bottom: 0;
    height: 50px;
    width: 50px;
    border-radius: 50%;
    border: none;
    color: white;
    background-color: $color-brand;
    user-select: none;
    cursor: pointer;
    p {
      font-size: xx-large;
      margin: auto;
    }
  }
  .floating-action-button:hover {
    transform: scale(1.1);
    filter: brightness(1.1);
  }
  .floating-action-button:active {
    filter: brightness(1);
    transform: scale(1);
  }
}

.workspace-card {
  @include shadow;
  border-radius: 8px;
  padding: 8px 10px;
  margin: 10px;
  cursor: pointer;
  user-select: none;
}
.workspace-card:hover {
  transform: scale(1.1);
}
.workspace-card:active {
  transform: scale(1);
}
</style>
