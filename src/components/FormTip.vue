<template>
    <div class="container">
        
        <p class="title"> Le/Tip <i class="fa-regular fa-money-bill-1 coin"> </i> </p>

        <div class="row">
            <div class="col-6" v-if="screen===true">

                <div class="input-group mb-3 field">
                    <div class="input-group-prepend">
                        <span class="word"> EUR </span>
                    </div>
                    
                    <div class="toggle">
                        <input type="checkbox" id="coin" v-model="coin">
                        <label for="coin"> </label>
                    </div>

                    <div class="input-group-append">
                        <span class="word"> USD </span>
                    </div>
                </div>


                <div class="input-group value">
                    <div class="input-group-prepend">
                        <label class="word"> Valor: </label> <label class="field"> {{ sign }} </label>
                    </div>
                    <div>
                        <input type="number" min="0" class="form-control" :value="bill ? bill : ''"  @input="onInput"/> 
                    </div>
                </div>
                
                
                <label class="word"> Gorjeta: </label> <label class="field"> {{ tip }}% </label>
                <div class="input-group mb-3 field">
                    <div class="input-group-prepend">
                        <span> 10 </span>
                    </div>
                    
                    <input type="range" name="tip" min="10" max="20" v-model.number="tip"/>
                    
                    <div class="input-group-append">
                        <span> 20 </span>
                    </div>
                </div>


                <label class="word"> Pessoas: </label> <label class="field"> {{ people }} </label>
                <div class="input-group mb-3 field">
                    <div class="input-group-prepend">
                        <span> 02 </span>
                    </div>
                    
                    <input type="range" name="people" min="2" max="16" v-model.number="people"/>
                    
                    <div class="input-group-append">
                        <span> 16 </span>
                    </div>
                </div>
            </div>
        

            <!-- Mobile -->
            <div class="col-6 desktop">
                <LeTip :bill="bill" :tip="tip" :people="people" :sign="sign"> </LeTip>
            </div>

            <div class="col-6 mobile">
                <LeTip :bill="bill" :tip="tip" :people="people" :sign="sign" v-if="screen===false"> </LeTip>
            </div>

            <i class="fa-solid fa-arrow-right mobile arrow" v-on:click="right" v-if="screen===true"> </i>
            <i class="fa-solid fa-arrow-left mobile arrow"  v-on:click="left"  v-if="screen===false"> </i>


        </div>

    </div>

</template>


<script>

    import LeTip from './LeTip';
    
    export default 
    {
        components: 
        {
            LeTip
        },
        data()
        {
            return {
                bill: 0,
                tip: 10,
                people: 2,
                coin: false,
                screen: true
            }
        },
        computed: 
        {
            sign() 
            {
                if(this.coin == true)
                {
                    return '$'
                }
                return '€'
            }
        },
        methods:
        {
            right()
            {
                this.screen = false
            },
            left()
            {
                this.screen = true
            },
            onInput(event) 
            {
                const value = event.target.value;

                this.bill = value > 0 ? parseFloat(value) : 0.00
            }
        }
    }

</script>


<style>

    .container
    {
        height: 100vh;
        background: #FFFFFF;
        border-radius: 8px;
    }

    .position
    {
        position: fixed;
    }

    .coin
    {
        font-size: 35px;
        color: #000000;
    }
    
    .title
    {
        font-size: 33px;
        color: #000000;
        font-weight: bold;
        text-align: center;
        border: 2px solid #02cc02;
        border-radius: 8px;
        padding: 4px;
    }

    .row 
    {
        margin-top: 30px;
    }

    .field
    {
        margin-top: 1%;
        color: #000000;
        font-size: 18px;
    }

    .input-group-prepend
    {
        margin-right: 10px;
    }

    .toggle
    {
        margin-top: 3px;
        margin-right: 10px;
    }

    .toggle > input 
    {
        display: none;
    }

    .toggle > label 
    {
        position: relative;
        display: block;
        height: 20px;
        width: 44px;
        background: #898989;
        border-radius: 100px;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    .toggle > label:after 
    {
        position: absolute;
        left: -2px;
        top: -3px;
        display: block;
        width: 26px;
        height: 26px;
        border-radius: 100px;
        background: #cac9c9;
        box-shadow: 0px 3px 3px rgba(0,0,0,0.05);
        content: '';
        transition: all 0.3s ease;
    }

    .toggle > label:active:after 
    {
        transform: scale(1.15, 0.85);
    }

    .toggle > input:checked ~ label
    {
        background: #92f392;
    }
    
    .toggle > input:checked ~ label:after 
    {
        left: 20px;
        background: #02cc02;
    }
    
    .toggle > input:disabled ~ label 
    {
        background: #d5d5d5;
        pointer-events: none;
    }
    
    .toggle > input:disabled ~ label:after 
    {
        background: #bcbdbc;
    }

    .value
    {
        margin-top: 5%;
    }

    .word
    {
        color: #000000;
        margin-top: 5%;
        font-size: 18px;
        font-weight: bold;
    }

    .form-control
    {
        border: 2px solid #02cc02;
    }

    input[type='range'],
    input[type='range']::-webkit-slider-runnable-track,
    input[type='range']::-webkit-slider-thumb 
    {
        -webkit-appearance: none;
    }

    input[type='range'] 
    {
        margin-top: 10px;
        margin-right: 10px;
        width: 47%;
        height: 0.5em;
        border-radius: 30px !important;
        background-color: #f5f5cb;
        border: 2px solid #02cc02;
    }

    input[type='range']::-webkit-slider-thumb 
    {
        width: 20px;
        height: 20px;
        margin-top: -4.5px;
        background-color: #02cc02;
        border-radius: 60px;
    }

    .arrow
    {
        padding-right: 20px;
        padding-bottom: 20px;
        position: absolute;
        text-align: right;
        bottom: 0px;
        font-size: 25px;
        color: #000000;
    }

    .desktop 
    {
        display: block;
    }

    .mobile 
    {
        display: none;
    }

    @media(max-width: 480px) 
    {
        .desktop
        {
            display: none;
        }
        .mobile 
        {
            display: block;
        }
    }
    
</style>