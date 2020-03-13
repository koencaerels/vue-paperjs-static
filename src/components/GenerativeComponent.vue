<template>
    <div id="Component_GenerativeComponent">
        <div id="canvas-wrapper">
            <canvas id="canvas-paper" width="800" height="600" ref="canvas" hidpi="off"></canvas>
        </div>
        <div id="toolbar">
            <div class="columns has-text-centered">
                <div class="column">
                    <b-button type="is-dark" @click="generate()">generate another</b-button>
                </div>
                <div class="column">
                    <a id="downloadPng" download="file.png">
                        <b-button type="is-dark" @click="saveAsPng()">save as .png</b-button>
                    </a>
                </div>
                <div class="column">
                    <a id="downloadSvg" download="file.svg">
                        <b-button type="is-dark" @click="saveAsSvg()">save as .svg</b-button>
                    </a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    import paper from 'paper';
    // import view from 'paper';

    export default {
        name: "GenerativeComponent"
        , props: []
        , components: {}
        , mounted() {
            paper.install(window);
            paper.setup(this.$refs.canvas);
            this.pCanvas = new paper.Project(this.$refs.canvas);
            this.init();
        }
        , data() {
            return {
                pCanvas: null
                , radius: 150
                , titleWork: "title of your work"
            }
        }
        , watch: {}
        , methods: {
            init() {
                this.renderComposition();
            }
            , generate() {
                this.pCanvas.clear();
                this.radius += 10;
                this.renderComposition();
            }
            , renderComposition() {
                // --------------------------------------------------------------------------------- render a background
                let _start = new paper.Point(0, 0);
                let _end = new paper.Point(800, 600);
                let _background = new paper.Path.Rectangle(_start, _end);
                _background.fillColor = new paper.Color(0.9, 0.9, 0.9);
                // ------------------------------------------------------------------------------------- render the work
                let center = new paper.Point(400, 300);
                let color = paper.Color.random();
                new paper.Path.Circle({
                    center: center
                    , radius: this.radius
                    , fillColor: color
                });
                // ------------------------------------------------------------------------------------ titling the work
                let text = new paper.PointText(new paper.Point(400, 550));
                text.justification = 'center';
                text.fillColor = 'gray';
                text.fontFamily = 'monospace';
                text.fontSize = 20;
                text.content = '"' + this.titleWork + '"';
            }
            , saveAsPng() {
                let link = document.getElementById("downloadPng");
                let image = this.$refs.canvas.toDataURL("image/png").replace("image/png", "image/octet-stream");
                link.setAttribute("href", image);
                link.setAttribute("download", this.titleWork + ".png");
            }
            , saveAsSvg() {
                let url = "data:image/svg+xml;utf8," + encodeURIComponent(this.pCanvas.exportSVG({asString: true}));
                let link = document.getElementById("downloadSvg");
                link.setAttribute("href", url);
                link.setAttribute("download", this.titleWork + ".svg");
            }
        }
        , computed: {}
    }
</script>

<style scoped>

    #Component_GenerativeComponent {
        width: 800px;
        margin-left: auto;
        margin-right: auto;
        padding-top: 2em;
    }

    #canvas-wrapper {
        background-color: #efefef;
        display: block;
        width: 800px;
        height: 600px;
    }

    #toolbar {
        padding-top: 2em;
    }

</style>
