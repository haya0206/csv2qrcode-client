<script>
  import axios from "axios";
  let dragZoneText = "Drag csv file to this Drop Zone ...";
  async function dropHandler(ev) {
    console.log("File(s) dropped");

    // Prevent default behavior (Prevent file from being opened)
    ev.preventDefault();
    try {
      let file = null;
      if (ev.dataTransfer.items) {
        if (ev.dataTransfer.items.length > 1) {
          throw "Multiple files are not supported..";
        }
        dragZoneText = "In process...";
        file = ev.dataTransfer.items[0].getAsFile();
      } else {
        if (ev.dataTransfer.items.length > 1) {
          throw "Multiple files are not supported..";
        }
        dragZoneText = "In process...";
        // Use DataTransfer interface to access the file(s)
        file = ev.dataTransfer.files[0];
      }
      const url =
        "https://f07y75cjfj.execute-api.ap-northeast-2.amazonaws.com/production/qrcode";
      const formData = new FormData();
      formData.append("file", file);
      formData.append("img", "png");
      const config = {
        headers: {
          "content-type": "multipart/form-data"
        }
      };
      const base64 = (await axios.post(url, formData, config)).data;
      saveAs(
        "data:application/zip;base64," + base64,
        `${Math.random()
          .toString(36)
          .substring(7)}.zip`
      );
    } catch (e) {
      dragZoneText = e;
    }
  }

  function dragOverHandler(ev) {
    console.log("File(s) in drop zone");

    // Prevent default behavior (Prevent file from being opened)
    ev.preventDefault();
  }

  function dragEnterHandler(ev) {
    dragZoneText = "Drop!";
  }

  function dragLeaveHandler(ev) {
    dragZoneText = "Drag csv file to this Drop Zone ...";
  }

  function saveAs(uri, filename) {
    var link = document.createElement("a");
    if (typeof link.download === "string") {
      link.href = uri;
      link.download = filename;

      //Firefox requires the link to be in the body
      document.body.appendChild(link);

      //simulate click
      link.click();

      //remove the link when done
      document.body.removeChild(link);
    } else {
      window.open(uri);
    }
  }
</script>

<style>
  #drop_zone {
    display: flex;
    justify-content: center;
    width: 500px;
    height: 200px;
    border: 1px solid #000;
    border-style: dashed;
  }
  #wrapper {
    display: flex;
    justify-content: center;
  }
</style>

<div id="wrapper">
  <div
    id="drop_zone"
    on:drop|preventDefault={dropHandler}
    on:dragenter|preventDefault={dragEnterHandler}
    on:dragleave|preventDefault={dragLeaveHandler}
    on:dragover|preventDefault={dragOverHandler}>
    <p>{dragZoneText}</p>
  </div>
</div>
