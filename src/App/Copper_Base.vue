<template>
    <section>
        <div class="Image_Preview_HolderContainer" id="previewImage_Parent">
            <div
            class="Move_Cropper_Div"
            id="Move_Cropper_Div"
            v-on="cropperMovement">
                <div class="Resizer TL" v-on="{ mousedown: resizeMoverMouseDown, touchstart: resizeMovertuouchStart}"></div>
                <div class="Resizer TR" v-on="{ mousedown: resizeMoverMouseDown, touchstart: resizeMovertuouchStart}"></div>
                <div class="Resizer BL" v-on="{ mousedown: resizeMoverMouseDown, touchstart: resizeMovertuouchStart}"></div>
                <div class="Resizer BR" v-on="{ mousedown: resizeMoverMouseDown, touchstart: resizeMovertuouchStart}"></div>
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
            RatioOne: 1,
            RatioTwo: 1,
            cropperMovement: {
                mousedown: this.mousedown,
                touchstart: this.touchstart
            },
        }
    },

    methods:{

        // =============[ FOR LARGE SCREEN DEVICE ]==============
        mousedown(e){
            let el1 = document.querySelector("#previewImage_Parent")
            let el2 = document.querySelector(".Move_Cropper_Div")
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
                    rect2.x = rect1.x + 1
                }
                if ((rect2.x+rect2.width) > (rect1.x + rect1.width)){
                    rect2.x = ((rect1.x + rect1.width) - rect2.width) - 4
                }
                if (rect2.y < rect1.y){
                    rect2.y = rect1.y + 1
                }
                if ((rect2.y+rect2.height) > (rect1.y + rect1.height)){
                    rect2.y = ((rect1.y + rect1.height) - rect2.height) - 1
                }

                el2.style.left = rect2.left - newX + "px";
                el2.style.top = rect2.top - newY + "px";

                prevX = e.clientX;
                prevY = e.clientY;
            }

            // REMOVE EVENT AFTER MOUSE-UP
            function mouseup(){
                window.removeEventListener("mousemove", mousemove);
                window.removeEventListener("mouseup", mouseup);
                this.isMoveble = false;
            }
        },
        // :~~~:~~~:[ RESIZE MOVEBLE DIV BY MOUSE-DOWN ]:~~~:~~~: 
        resizeMoverMouseDown(e){
            let currentResizer = e.target;
            let ratioOne = this.RatioOne;
            let ratioTwo = this.RatioTwo
            let el1 = document.querySelector("#previewImage_Parent")
            let el2 = document.querySelector(".Move_Cropper_Div")
            window.addEventListener('mousemove', mousemove);
            window.addEventListener('mouseup', mouseup);
            let prevY = e.clientY;

            function mousemove(e){
                const rect1 = el1.getBoundingClientRect();
                const rect2 = el2.getBoundingClientRect();
                
                el2.style.maxHeight = `${rect1.height}px`;
                el2.style.maxWidth= `${rect1.width}px`;

                if (currentResizer.classList.contains("BR")){
                    let resizerVariable = rect2.height - (prevY - e.clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                else if (currentResizer.classList.contains("BL")){
                    let resizerVariable = rect2.height - (prevY - e.clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                else if (currentResizer.classList.contains("TR")){
                    let resizerVariable = rect2.height + (prevY - e.clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                else if(currentResizer.classList.contains("TL")){
                    let resizerVariable = rect2.height + (prevY - e.clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                prevY = e.clientY;

            }
            function mouseup(){
                window.removeEventListener('mousemove', mousemove);
                window.removeEventListener('mouseup', mouseup);
            }
        },

        // =============[ FOR SMALL SCREEN DEVICE ]==============
        touchstart(e){
            this.isMoveble = true;
            let el1 = document.querySelector("#previewImage_Parent")
            let el2 = document.querySelector(".Move_Cropper_Div")
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
                    rect2.x = rect1.x + 1
                }
                if ((rect2.x+rect2.width) > (rect1.x + rect1.width)){
                    rect2.x = ((rect1.x + rect1.width) - rect2.width) - 4
                }
                if (rect2.y < rect1.y){
                    rect2.y = rect1.y + 1
                }
                if ((rect2.y+rect2.height) > (rect1.y + rect1.height)){
                    rect2.y = ((rect1.y + rect1.height) - rect2.height) - 1
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
        // :~~~:~~~:[ RESIZE MOVEBLE DIV BY TOUCH ]:~~~:~~~: 
        resizeMovertuouchStart(e){
            let currentResizer = e.target;
            let ratioOne = this.RatioOne;
            let ratioTwo = this.RatioTwo;
            let el1 = document.querySelector("#previewImage_Parent")
            let el2 = document.querySelector(".Move_Cropper_Div")
            window.addEventListener('touchmove', touchmove);
            window.addEventListener('touchend', touchend);
            let prevX = e.touches[0].clientX;
            let prevY = e.touches[0].clientY;

            console.warn(currentResizer)
            console.warn(prevX)
            console.warn(prevY)

            function touchmove(e){
                console.log(e)
                const rect1 = el1.getBoundingClientRect();
                const rect2 = el2.getBoundingClientRect();
                
                el2.style.maxHeight = `${rect1.height}px`;
                el2.style.maxWidth= `${rect1.width}px`;
                console.log(rect1)
                console.log(rect2)
                console.log(this.ratio)
                if (currentResizer.classList.contains("BR")){
                    let resizerVariable = rect2.height - (prevY - e.touches[0].clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                else if (currentResizer.classList.contains("BL")){
                    let resizerVariable = rect2.height - (prevY - e.touches[0].clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                else if (currentResizer.classList.contains("TR")){
                    let resizerVariable = rect2.height + (prevY - e.touches[0].clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                else if(currentResizer.classList.contains("TL")){
                    let resizerVariable = rect2.height + (prevY - e.touches[0].clientY)
                    el2.style.height =  `${ratioOne * resizerVariable}px`;
                    el2.style.width  = `${ratioTwo * resizerVariable}px`;
                }
                prevX = e.touches[0].clientX;
                prevY = e.touches[0].clientY;

            }
            function touchend(){
                window.removeEventListener('touchmove', touchmove);
                window.removeEventListener('touchend', touchend);
            }
        },


        // =============[ IMAGE PREVIEW BEFORE UPLOAD ]==============
        imgFileSelected(event){

            const File = event.target.files[0]

            if (event.target!=null){
                this.FileName = event.target.files[0].name
            }
            if (File){
                const reader = new FileReader();

                reader.addEventListener("load", function(){
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
}
.Image_Preview_HolderContainer img{
    width: 100%;
    height: 100%;
    max-width: 400px;
    max-height: 400px;
    box-sizing: border-box;
    object-fit: contain;
}
.Move_Cropper_Div{
    position: absolute;
    min-height: 100px;
    min-width: 100px;
    border: 1px solid rgb(255, 255, 255);
    border-style: dashed;
    box-sizing: border-box;
    background: transparent;
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
    cursor: se-resize;
}
.TR{
    top: -4px;
    right: -4px;
    cursor: ne-resize;
}
.BL{
    bottom: -4px;
    left: -4px;
    cursor: sw-resize;
}
.BR{
    bottom: -4px;
    right: -4px;
    cursor: se-resize
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