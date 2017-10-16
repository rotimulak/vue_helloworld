<template>
    <div class="page">
        <div id="dimmer" v-on:click="hideproperty()">
        </div>
        <div class="app-header">
            <div class="app-name">
                Tree DB Schema Editor
            </div>
            <div class="app-btn">
                <a href=#  class="app-btn">Import</a> <a href=#  class="app-btn">Export</a>
            </div>
        </div>

        <div class="ext-container">
            <div class="int-container">
                <div class="entity" v-bind:id="'object-' + obj.name" v-for="obj in entities">
                    <input type="text" class="entity-header" v-bind:value="obj.name">

                    <div v-for="entity in obj.fields" class="entity-field" v-bind:id="entity.field">
                        <input type="text" class="entity-field-name" v-bind:value="entity.field"> 
                        <div class="entity-field-type" v-on:click="showproperty(entity, obj)" v-bind:id="'type-' + entity.field + '-' + entity.type">{{entity.type}}</div>
                    </div>

                    <div class="entity-add-field">
                        <div class="entity-field-plus" v-on:click="addfield(obj)">
                            +
                        </div>
                    </div>
                </div>

                <div class="entity-plus" v-on:click="addentity()">
                    +
                </div>
            </div>

            <div id="properties">
                <div id="p-name"></div>
                <form>
                    <p v-for="rt in reftypes">
                        <input type="radio" 
                            v-bind:id="'rt-' + rt" 
                            v-bind:value="rt" 
                            name="selecttype"
                            v-model="reftypechecked"
                            >
                        <label v-bind:for="'rt-' + rt"> {{rt}}</label>
                    </p>
                    <hr>
                    <p v-for="refentity in entities">
                        <input type="radio" 
                            v-bind:id="'3rd-'+refentity.name" 
                            v-bind:value="refentity.name" 
                            v-model="reftypechecked"
                            name="selecttype">
                        <label v-bind:for="'3rd-'+refentity.name"> {{refentity.name}} </label> 
                    </p>
                        <!-- <select size="1" class="entity-field-type">
                            <option value="makearray" class="dropdown">[Make it array]</option>
                            <option value="createentity" class="dropdown">Crate New Entity</option>
                        </select> -->
                </form>
                <div id="p-fun" v-on:click="deletefield(pentity, pfield)">Delete entity</div>
                    <!--  -->

            </div>

        </div>


    </div>
</template>

<script>

    export default {

        name: 'app',
        data() {
           
            return {

                reftypechecked: [],
                pentity: {}, 
                pfield: {},

                reftypes: [ 'string', 'int', 'float'],

                entities: [
                    {
                        name: "Drivers",
                        fields: [{field: "name", type: "string"},
                                 {field: "surname", type: "string"},
                                 {field: "fleet", type: "Fleet"}
                        ]
                    },
                    {
                        name: "Fleet",
                        fields: [{field: "car", type: "string"},
                                 {field: "power", type: "int"},
                                 {field: "driver", type: "Drivers"}
                        ]
                    }
                ]
            }
        },
        watch: {
            reftypechecked (v) {
                console.log(v)
            }
        },
        methods: {
            // fun() {
            //     return 5;
            // },

            addfield: function(object) {
                object.fields.push( {field: "", type: "string"} )
                console.log('huray')
            },
            deletefield: function(pentity, pfield) {

                var ientity = this.entities.indexOf(pentity);
                if (ientity > -1) {
                    var ifield = this.entities(ientity).fields.indexOf(pfield)
                    if (ifield > -1) {
                        this.entities(ientity).fields.splice(ifield, 1);
                    }
                }
                console.log( pentity, pfield, ientity, ifield )

            },

            hideproperty: function() {
                document.getElementById("properties").style.display='none';
                document.getElementById("dimmer").style.visibility='hidden';
                document.getElementById("dimmer").style.opacity = "0";
            },

            showproperty: function(entity, obj) {
                // console.log(entity,obj)
                this.reftypechecked = entity.type;
                this.pentity = entity;
                this.pfield = entity.field;
                console.log(this.reftypechecked, entity.type);
                document.getElementById("properties").style.display = "block";
                document.getElementById("dimmer").style.visibility = "visible";
                document.getElementById("dimmer").style.opacity = "1";
                new Tether({
                    element: $("#properties"),
                    target:  $("#type-"+entity.field + '-' + entity.type),
                    attachment: 'middle left',
                    targetAttachment: 'top right'
                });
                document.getElementById("p-name").innerText = entity.field;
            },

            addentity: function() {
                console.log('huray')
                this.entities.push( { name: "new" , fields: [] } )
            }
        }

    }
</script>

<style>
    body, html {
        /* background-color: #263238; */
        margin: 0;
        padding: 0;
        font-size: 16px;
        font-family: 'Open Sans', sans-serif;
    }
    #dimmer {
        visibility: hidden;
        opacity: 0;

        position: absolute;
        top: 0;
        left: 0;
        height: 100%;
        width: 100%;
        background-color: rgba(0,0,0,0.7);
        z-index: 10;
        transition: ease-in-out 0.25s;
    }

    .page {
        height: 100vh;
    }

    .dropdown {
        font-size: 1rem;
        padding: 1rem;
    }

    #properties {
        background-color: white;
        padding: 0.25rem 1rem;
        border-radius: 6px;
        box-shadow: 0px 3px 3px rgba(0,0,0,0.32);
        /* visibility: hidden; */
        display: none;
        z-index: 20;
    }

    .app-header {
        height: 4rem;
        line-height: 4rem;
        padding-left: 1rem;
        background-color: #263238;
        font-size: 1.125rem;
        font-weight: normal;
        color: white;

        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-flex-direction: row;
        -ms-flex-direction: row;
        flex-direction: row;
        -webkit-justify-content: space-between;
        -ms-flex-pack: justify;
        justify-content: space-between;
        -webkit-align-content: flex-start;
        -ms-flex-line-pack: start;
        align-content: flex-start;
        -webkit-align-items: center;
        -ms-flex-align: center;
        align-items: center;
    }

    .app-name {
        width: max-content;
    }

    .app-btns {
        width: max-content;
    }

    .app-btn {
        color: #536dfe;
        margin: 0 1.5rem;
        text-decoration: none;
    }

    .app-btn:hover {
        color: #8c9eff;
        cursor: pointer;
    }

    .ext-container {
        background-color: #cfd8dc;
        height: calc(100% - 4rem);

        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-flex-direction: column;
        -ms-flex-direction: column;
        flex-direction: column;
        -webkit-flex-wrap: nowrap;
        -ms-flex-wrap: nowrap;
        flex-wrap: nowrap;
        -webkit-justify-content: center;
        -ms-flex-pack: center;
        justify-content: center;
        -webkit-align-content: center;
        -ms-flex-line-pack: center;
        align-content: center;
        -webkit-align-items: center;
        -ms-flex-align: center;
        align-items: center;

        overflow: auto;
    }

    .int-container {
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-flex-direction: row;
        -ms-flex-direction: row;
        flex-direction: row;

        -webkit-flex-wrap: nowrap;
        -ms-flex-wrap: nowrap;
        flex-wrap: nowrap;

        -webkit-justify-content: center;
        -ms-flex-pack: center;
        justify-content: center;

        -webkit-align-content: flex-start;
        -ms-flex-line-pack: start;
        align-content: flex-start;

        -webkit-align-items: flex-start;
        -ms-flex-align: start;
        align-items: flex-start;
    }

    .entity {
        -webkit-align-self: auto;
        -ms-flex-item-align: auto;
        align-self: auto;
        width: 12rem;
        background-color: white;
        box-shadow: 0px 2.5px 5px rgba(0, 0, 0, 0.36);
        border-radius: 0.75rem;
        margin: 0 1.5rem;
    }

    .entity-header {
        font-size: 1.125rem;
        font-weight: normal;
        line-height: 3rem;
        color: white;
        height: 3rem;
        padding-left: 1rem;
        background-color: #3f51b5;
        border-radius: 0.75rem 0.75rem 0 0;
        border: 0;
        width: calc(100% - 1rem);
    }

    .entity-header:focus {
        outline: none;
    }

    .entity-header:hover {
        background-color: #283593;
        cursor: pointer;
    }

    .entity-plus {
        color: white;
        background-color: #3f51b5;
        padding: 0.825rem 1.25rem;
        border-radius: 1.5rem;
        text-align: center;
        margin-left:1rem;
        box-shadow: 0px 2.5px 5px rgba(0, 0, 0, 0.36);
    }
    .entity-plus:hover {
        background-color: #283593;
        cursor: pointer;
    }



    .entity-field:not(:last-child) {
        height: 4rem;
        border-bottom: 1px solid #cfd8dc;
    }

    .entity-field:hover {
        background-color: #b3e5fc;
        cursor: pointer;
    }

    .entity-field-name {
        font-size: 1.125rem;
        font-weight: normal;
        color: black;
        background: transparent;
        height: 2rem;
        padding: 0.25rem 0 0 1rem;
        border: 0;
        width: calc(100% - 1rem);
    }

    .entity-field-name:focus {
        outline: none;
    }

    .entity-field-type {

        line-height: 1.25rem;
        font-size: 0.6875rem;
        font-weight: normal;
        color: #90a4ae;
        /* height: 1.75rem; */
        /* padding: 1rem; */
        padding: 0.25rem 1rem;
        text-transform: uppercase;
        letter-spacing: 1px;
        border: 0;
        background: transparent;
        /* margin: 0 0.5rem; */
        /* width: calc(100% - 1rem); */
        /* width: calc(100%); */
    }

    .entity-field-type:focus {
        outline: none;
    }

    .referal-type {
        color: #3949ab;
        font-weight: bold;
    }

    .referal-type:hover {
        color: #283593;
        text-decoration: underline;
        text-decoration-style: wavy;
    }

    .entity-add-field {
        line-height: 3rem;
        font-size: 1.125rem;
        font-weight: bold;
        color: #3f51b5;
        height: 3rem;
        /* padding-left: 1rem; */
        text-align: center;
        border-radius: 0 0 0.75rem 0.75rem;
    }

    .entity-add-field:hover {
        background-color: #b3e5fc;
        cursor: pointer;
    }
</style>
