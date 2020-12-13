<template>
    <section>
        <div class="Image_Preview_HolderContainer" id="previewImage_Parent">
            <div
            class="Move_Resize_Div"
            id="Move_Resize_Div"
            v-on="smallScreenMovement">
                <div class="Resizer TL"></div>
                <div class="Resizer TR"></div>
                <div class="Resizer BL"></div>
                <div class="Resizer BR"></div>
            </div>
            <img id="previewImage">
        </div>
        <div class="FileSelectContainer">
            <label for="FileSelector">
                Select |
            </label>
            <p v-if="(FileName != null)">{{FileName}}</p>
            
            <input type="file" @change="imgFileSelected" id="FileSelector">
        </div>
    </section>
</template>


<script>

export default {
    name: 'Copper_Base',
    data(){
        return{
            FileName: null,
            smallScreenMovement: {
                mousedown: this.mousedown,
                touchstart: this.touchstart
            }
        }
    },
    methods:{

        // =============[ FOR LARGE SCREEN DEVICE ]==============
        mousedown(e){
            let el1 = document.querySelector("#previewImage_Parent")
            let el2 = document.querySelector(".Move_Resize_Div")
            window.addEventListener("mousemove", mousemove);
            window.addEventListener("mouseup", mouseup);

            let prevX = e.clientX;
            let prevY = e.clientY;

            // ADD EVENT AFTER MOUSE-MOVE
            function mousemove(e){
                let newX = prevX - e.clientX;
                let newY = prevY - e.clientY;

                // GET RECTANGLE PROPERTIS (HEIGHT, WIDTH, X, Y)
                const rect1 = el1.getBoundingClientRect();
                const rect2 = el2.getBoundingClientRect();

                if (rect2.x < rect1.x){
                    rect2.x = rect1.x + 2
                }
                if ((rect2.x+rect2.width) > (rect1.x + rect1.width)){
                    rect2.x = ((rect1.x + rect1.width) - rect2.width) - 2
                }
                if (rect2.y < rect1.y){
                    rect2.y = rect1.y + 2
                }
                if ((rect2.y+rect2.height) > (rect1.y + rect1.height)){
                    rect2.y = ((rect1.y + rect1.height) - rect2.height) - 2
                }

                el2.style.left = rect2.left - newX + "px";
                el2.style.top = rect2.top - newY + "px";

                console.log(rect1)
                console.log(rect2)

                prevX = e.clientX;
                prevY = e.clientY;
            }

            // REMOVE EVENT AFTER MOUSE-UP
            function mouseup(){
                window.removeEventListener("mousemove", mousemove);
                window.removeEventListener("mouseup", mouseup);
            }
        },

        // =============[ FOR SMALL SCREEN DEVICE ]==============
        touchstart(e){
            let el1 = document.querySelector("#previewImage_Parent")
            let el2 = document.querySelector(".Move_Resize_Div")
            window.addEventListener("touchmove", touchmove);
            window.addEventListener("touchend", touchend);

            let prevX = e.touches[0].clientX;
            let prevY = e.touches[0].clientY;

            // START TUCH EVENT WHEN THE USER TUCH THE ELEMENT
            function touchmove(e){
                let newX = prevX - e.touches[0].clientX;
                let newY = prevY - e.touches[0].clientY;

                // GET RECTANGLE PROPERTIS (HEIGHT, WIDTH, X, Y)
                const rect1 = el1.getBoundingClientRect();
                const rect2 = el2.getBoundingClientRect();

                if (rect2.x < rect1.x){
                    rect2.x = rect1.x + 2
                }
                if ((rect2.x+rect2.width) > (rect1.x + rect1.width)){
                    rect2.x = ((rect1.x + rect1.width) - rect2.width) - 2
                }
                if (rect2.y < rect1.y){
                    rect2.y = rect1.y + 2
                }
                if ((rect2.y+rect2.height) > (rect1.y + rect1.height)){
                    rect2.y = ((rect1.y + rect1.height) - rect2.height) - 2
                }

                el2.style.left = rect2.left - newX + "px";
                el2.style.top = rect2.top - newY + "px";
                
                prevX = e.touches[0].clientX;
                prevY = e.touches[0].clientY;
            }
            
            // REMOVE TUCH EVENT AFTER REMOVE FINGER FROM ELEMENT
            function touchend(){
                window.removeEventListener("touchmove", touchmove);
                window.removeEventListener("touchend", touchend);
            }
        },


        // =============[ IMAGE PREVIEW BEFORE UPLOAD ]==============
        imgFileSelected(event){
            // console.warn(event)
            // console.warn(event.target.files[0].name)
            // console.warn(event.target.result)

            const File = event.target.files[0]

            if (event.target!=null){
                this.FileName = event.target.files[0].name
            }
            if (File){
                const reader = new FileReader();
                console.warn(reader)
                reader.addEventListener("load", function(){
                    // console.warn(this)
                    // console.warn(this.result)
                    // console.warn(previewImage)
                    let previewImage = document.getElementById('previewImage');
                    previewImage.setAttribute('src', this.result)
                });
                reader.readAsDataURL(File);
            }
            event.preventDefault();
        }
    }
}
</script>

<style scoped>
section{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    overflow: hidden;
}
.Image_Preview_HolderContainer{
    width: 100%;
    min-height: 100px;
    max-width: 400px;
    max-height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid orangered;
    box-sizing: border-box;
    overflow: hidden;
    /* filter: blur(2px); */
}
.Image_Preview_HolderContainer img{
    width: 100%;
    height: 100%;
    max-width: 400px;
    max-height: 400px;
    box-sizing: border-box;
    object-fit: contain;
}
.Move_Resize_Div{
    position: absolute;
    min-height: 100px;
    min-width: 100px;
    border: 1px solid rgb(255, 255, 255);
    border-style: dashed;
    box-sizing: border-box;
    box-shadow: 0px 0px 3px rgb(0, 0, 0);
}

/* RESIZE THIS MOVEBLE ELEMENT */
.Resizer{
    position: absolute;
    height: 8px;
    width: 8px;
    background: rgba(0, 0, 0, 0.719);
    z-index: 2;
}
.TL{
    top: -4px;
    left: -4px;
    cursor: nesw-resize;
}
.TR{
    top: -4px;
    right: -4px;
    cursor: nesw-resize;
}
.BL{
    bottom: -4px;
    left: -4px;
    cursor: nesw-resize;
}
.BR{
    bottom: -4px;
    right: -4px;
    cursor: nesw-resize; 
}

/* FILE SELECTOR CONTAINER */
.FileSelectContainer{
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
}
#FileSelector{
    /* border: 1px solid green; */
    display: none;
}
</style>