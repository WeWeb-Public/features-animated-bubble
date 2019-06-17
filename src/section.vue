<!-- This is a Vue.js single file component. -->
<!-- Check the Vue.js doc here :  -->
<!-- https://vuejs.org/v2/guide/ -->

<!-- This is your HTML -->
<template>
    <div class="feature-animated-bubble" :style="customStyle">
        <!-- wwManager:start -->
        <wwSectionEditMenu v-bind:sectionCtrl="sectionCtrl" :options="openOptions"></wwSectionEditMenu>
        <!-- wwManager:end -->
        <wwObject class="background" tag="div" :ww-object="section.data.background" ww-category="background"></wwObject>
        <!--TOP WWOBJS-->
        <div class="list-container">
            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.topWwObjs" class="list" @ww-add="add(section.data.topWwObjs, $event)" @ww-remove="remove(section.data.topWwObjs, $event)">
                <wwObject tag="div" v-for="object in section.data.topWwObjs" :key="object.uniqueId" :ww-object="object"></wwObject>
            </wwLayoutColumn>
        </div>

        <div class="feature-container">
            <div class="left-block" :style="{'z-index': `${section.data.features.length + 1}`}">
                <wwObject class="background" tag="div" :ww-object="section.data.backgroundLeft" ww-category="background"></wwObject>

                <div class="left-items">
                    <wwObject class="background" tag="div" :ww-object="section.data.backgroundLeft" ww-category="background"></wwObject>
                    <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.leftItems" class="list" @ww-add="add(section.data.leftItems, $event)" @ww-remove="remove(section.data.leftItems, $event)">
                        <wwObject tag="div" v-for="object in section.data.leftItems" :key="object.uniqueId" :ww-object="object"></wwObject>
                    </wwLayoutColumn>
                </div>
            </div>
            <div class="right-block" v-ww-visible="runAnim" ww-visible-offset="-200">
                <div class="feature feature-hidden" :data-index="index" v-for="(feature, index) in section.data.features" :key="feature.index" :style="{'transition-delay': `${index * 300}ms`, 'z-index': `${section.data.features.length - index}`}">
                    <div class="icon-container">
                        <!-- wwManager:start -->
                        <wwContextMenu tag="div" class="contextmenu" v-if="editMode" @ww-add-before="addFeatureBefore(index)" @ww-add-after="addFeatureAfter(index)" @ww-remove="removeFeature(index)">
                            <div class="wwi wwi-config"></div>
                        </wwContextMenu>
                        <!-- wwManager:end -->
                        <wwObject class="feature-icon" tag="div" :ww-object="feature.icon"></wwObject>
                    </div>
                    <div class="text-container">
                        <wwObject tag="div" :ww-object="feature.text"></wwObject>
                    </div>
                </div>
            </div>
        </div>

        <!--BOTTOM WWOBJS-->
        <div class="list-container">
            <wwLayoutColumn tag="div" ww-default="ww-text" :ww-list="section.data.bottomWwObjs" class="list" @ww-add="add(section.data.bottomWwObjs, $event)" @ww-remove="remove(section.data.bottomWwObjs, $event)">
                <wwObject tag="div" v-for="object in section.data.bottomWwObjs" :key="object.uniqueId" :ww-object="object"></wwObject>
            </wwLayoutColumn>
        </div>
    </div>
</template>

<!-- This is your Javascript -->
<!-- ✨ Here comes the magic ✨ -->
<script>
/* wwManager:start */
wwLib.wwPopups.addStory('FEATURE_ANIMATED-BUBBLE_CONFIG', {
    title: {
        en: 'Configuration',
        fr: 'Configuration'
    },
    type: 'wwPopupForm',
    storyData: {
        fields: [
            {
                label: {
                    en: 'Border Color:',
                    fr: 'Couleur des bordures :'
                },
                type: 'text',
                key: 'borderColor',
                valueData: 'borderColor',
                desc: {
                    en: 'Example: 2px solid #E53935',
                    fr: 'Exemple : 2px solid #E53935'
                }
            }
        ]
    },
    buttons: {
        NEXT: {
            text: {
                en: 'Ok',
                fr: 'Ok'
            },
            next: false
        }
    }
})
/* wwManager:end */
export default {
    name: "__COMPONENT_NAME__",
    props: {
        // The section controller object is passed to you.
        sectionCtrl: Object
    },
    data() {
        return {
        }
    },
    computed: {
        //Get the section object here !
        // It contains all the info and data about the section
        // Use it has you like !
        section() {
            return this.sectionCtrl.get();
        },
        editMode() {
            return this.sectionCtrl.getEditMode() == 'CONTENT'
        },
        customStyle() {
            return {
                '--borderColor': this.section.data.borderColor,
            }
        }

    },
    created() {

        //Initialize section data
        let needUpdate = false

        this.section.data = this.section.data || {};

        //Initialize content
        if (!this.section.data.background) {
            this.section.data.background = wwLib.wwObject.getDefault({
                type: 'ww-color',
                data: {}
            });
            needUpdate = true
        }

        if (!this.section.data.backgroundLeft) {
            this.section.data.backgroundLeft = wwLib.wwObject.getDefault({
                type: 'ww-color',
                data: {}
            });
            needUpdate = true
        }

        if (!this.section.data.topWwObjs) {
            this.section.data.topWwObjs = [];
            needUpdate = true;
        }

        if (!this.section.data.bottomWwObjs) {
            this.section.data.bottomWwObjs = [];
            needUpdate = true;
        }

        if (!this.section.data.leftItems) {
            this.section.data.leftItems = [];
            needUpdate = true;
        }

        if (!this.section.data.borderColor) {
            this.section.data.borderColor = '2px solid #E0294D';
            needUpdate = true;
        }
        if (!this.section.data.features) {
            this.section.data.features = [{
                icon: wwLib.wwObject.getDefault({
                    type: 'ww-icon',
                    data: {}
                }),
                text: wwLib.wwObject.getDefault({
                    type: 'ww-text',
                    data: {}
                })
            }, {
                icon: wwLib.wwObject.getDefault({
                    type: 'ww-icon',
                    data: {}
                }),
                text: wwLib.wwObject.getDefault({
                    type: 'ww-text',
                    data: {}
                })
            }];
            needUpdate = true;
        }

        if (needUpdate) {
            this.sectionCtrl.update(this.section);
        }
    },
    mounted() {
        // this.runAnim()
    },
    methods: {
        /* wwManager:start */
        add(list, options) {
            try {
                list.splice(options.index, 0, options.wwObject);
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        remove(list, options) {
            try {
                list.splice(options.index, 1);
                this.sectionCtrl.update(this.section);
            } catch (error) {
                wwLib.wwLog.error('ERROR : ', error);
            }
        },
        getNewFeature() {
            const card = JSON.parse(JSON.stringify(this.section.data.features[0]))
            wwLib.wwUtils.changeUniqueIds(card)
            return card
        },
        addFeatureBefore(index) {
            const newCard = this.getNewFeature()
            this.section.data.features.splice(index, 0, newCard);
            this.sectionCtrl.update(this.section);
        },
        addFeatureAfter(index) {
            const newCard = this.getNewFeature()
            this.section.data.features.splice(index + 1, 0, newCard);
            this.sectionCtrl.update(this.section);
            this.$nextTick(() => {
                this.runAnim()
            });
        },
        removeFeature(index) {
            if (!this.section.data.features.length) {
                return;
            }
            this.section.data.features.splice(index, 1);
            this.sectionCtrl.update(this.section);
        },

        async openOptions() {
            let options = {
                firstPage: 'FEATURE_ANIMATED-BUBBLE_CONFIG',
                data: {
                    borderColor: this.section.data.borderColor
                },
            }
            const result = await wwLib.wwPopups.open(options)
            let updateSection = false;
            if (result.borderColor) {
                this.section.data.borderColor = result.borderColor;
                updateSection = true;
            }
            if (updateSection) {
                this.sectionCtrl.update(this.section);
            }
            console.log(result);
        },


        /* wwManager:end */
        runAnim() {
            let features = this.$el.querySelectorAll('.feature')
            console.log('features :', features)
            for (let feature of features) {
                feature.classList.remove('feature-hidden')
            }

        }
    }
}
</script>

<!-- This is your CSS -->
<!-- Add "scoped" attribute to limit CSS to this component only -->
<!-- Add lang="scss" or others to use computed CSS -->
<style lang="scss" scoped>
.feature-animated-bubble {
    .background {
        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
    }

    .container {
        width: 100%;
        position: relative;
    }

    .section-subtitle {
        margin-top: 10px;
        margin-bottom: 20px;
    }

    .feature-container {
        display: -webkit-box;
        display: -webkit-flex;
        display: -ms-flexbox;
        display: flex;
        -webkit-flex-wrap: wrap;
        -ms-flex-wrap: wrap;
        flex-wrap: wrap;
        position: relative;
        margin-bottom: 50px;
    }

    .block {
        display: -webkit-box;
        display: -webkit-flex;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-orient: vertical;
        -webkit-box-direction: normal;
        -webkit-flex-direction: column;
        -ms-flex-direction: column;
        flex-direction: column;
    }

    .left-block {
        float: left;
        width: 100%;
        flex-basis: 100%;
        min-height: 100px;
        position: relative;
        border-bottom: var(--borderColor);
        box-shadow: 0 2px 80px 0 rgba(209, 209, 209, 0.5);
        z-index: 1;
        @media (min-width: 768px) {
            width: 40%;
            flex-basis: 40%;
            border-right: var(--borderColor);
            border-bottom: none;
        }
    }

    .left-items {
        position: relative;
        .list {
            width: 100%;
        }
    }

    .right-block {
        float: right;
        width: 100%;
        flex-basis: 100%;
        min-height: 20px;
        padding-bottom: 15px;
        @media (min-width: 768px) {
            width: 59%;
            flex-basis: 59%;
        }
        .feature {
            position: relative;
            margin-top: 25px;
            display: table;
            width: 100%;
            opacity: 1;
            -webkit-transition: all 0.5s ease;
            -moz-transition: all 0.5s ease;
            -o-transition: all 0.5s ease;
            transition: all 0.5s ease;
            @media (min-width: 768px) {
                margin-top: 15px;
                -webkit-transition: all 1s cubic-bezier(0.49, 0.69, 0, 0.96);
                -moz-transition: all 1s cubic-bezier(0.49, 0.69, 0, 0.96);
                -o-transition: all 1s cubic-bezier(0.49, 0.69, 0, 0.96);
                transition: all 1s cubic-bezier(0.49, 0.69, 0, 0.96);
            }
            &.feature-hidden {
                opacity: 0;
                transform: translateY(-100%);
                @media (min-width: 768px) {
                    transform: translateX(-100%);
                }
            }
            .icon-container {
                display: table-cell;
                vertical-align: middle;
                width: 40%;
                position: relative;
                @media (min-width: 768px) {
                    width: 30%;
                }
                &::before {
                    content: "";
                    height: 100%;
                    border-left: var(--borderColor);
                    position: absolute;
                    left: 50%;
                    top: -25px;
                    @media (min-width: 768px) {
                        content: "";

                        height: 0;
                        border-left: none;
                        position: absolute;
                        left: 0;

                        width: calc(100% - 30px);
                        border-top: var(--borderColor);
                        position: absolute;
                        top: 50%;
                    }
                }
            }
            .text-container {
                display: table-cell;
                vertical-align: middle;
                width: 60%;
                @media (min-width: 768px) {
                    width: 70%;
                }
            }
        }
    }

    .add-feature-button {
        display: inline-block;
        margin: 5px;
        width: auto;
    }

    .left-items {
        width: 100%;
        height: 100%;
        position: relative;
        display: flex;
        justify-items: center;
        align-items: center;
    }

    .list-container {
        position: relative;
    }

    .list {
        position: relative;
    }
    .feature-icon {
        float: right;
        margin-right: 25px;
        @media (min-width: 768px) {
        }
    }
    /* wwManager:start */
    .contextmenu {
        position: absolute;
        top: 0;
        left: 0;
        transform: translate(10px, 6px);
        width: 30px;
        height: 30px;
        color: white;
        background-color: #ef811a;
        border-radius: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.2rem;
        cursor: pointer;
        z-index: 1;
    }
    &.ww-editing {
        .feature-hidden {
            opacity: 1 !important;
            transform: none !important;
        }
    }
    /* wwManager:end */
}
</style>
