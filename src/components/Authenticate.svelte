<script>
  import { authHandlers } from "../store/store";

    let email = ""
    let password = ""
    let confirmPassword = ""
    let error = false
    let register = false
    let authenticating = false;

    async function handelAuthnticate(){
        if(authenticating){
            return;
        }
        
        if(!email || !password || (register  && !confirmPassword)){
            error = true
            return

        }
        authenticating = false;

        try {
            if(!register){
            await authHandlers.login(email,password)
             
        }
        else{
            await authHandlers.signup(email,password)
        }
            
        } catch (error) {
            console.log("It is an auth error!! ",error);
            error=true;
            
        }
       

        
    }

    function handleRegister (){
        register = !register


    }
</script>



<div class="authcontainer">

    <form action="">
        <h1>{register ? "Register" : "Login"}</h1>
        {#if error}
        <p class="error">Info added is not Correct</p>
        {/if}
        <label for="">
            <p class=" "></p>
            <input bind:value={email} type="email" placeholder="Email"/>

        </label>
        <label for="">
            <input bind:value={password} type="password" placeholder="Password"/>

        </label>
        {#if register}
        <label for="">
            <input bind:value={confirmPassword} type="password" placeholder="Confirm the password"/>

        </label>
        {/if}
        
        <button on:click={handelAuthnticate} type="button" class="submitbtn ">
            {#if authenticating}
            <i class="fa-solid fa-spinner spin"/>
            {:else}
            Submit
            {/if}
            </button>
    </form>
    <div class="options">
        <p>or</p>
        {#if register}
        <div>
            <p>Already have an acount?</p>
            <p on:click={handleRegister} on:keydown={()=>{}}>Login</p>
        </div>
        {:else}
        <div>
            <p>Don't have an acount?</p>
            <p on:click={handleRegister} on:keydown={()=>{}}>Register</p>
        </div>

        {/if}
    </div>
</div>

<style>
    .authcontainer {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        flex: 1;
        padding: 24px;

    }

    form{
        display: flex;
        flex-direction: column;
        gap:8px;
    }

    form,
    .options{
        width: 400px;
        max-width: 100%;
        margin: 0 auto;
    
    }

    form input{
        width: 100%;
        border: none;
        background: transparent;
        color: #ffffff;
        padding: 10px 15px;
    }

    h1{
        text-align: center;
        font-size: 3rem;
        color: #ffffff;
    }
    .error{
        text-align: center;
    }

    form label{
        position: relative;
        border: #ffffff;

    }
    button{
        font-size: 18px;
        background-color: #C5D8D7;
        width: 100%;
        padding: 10px 0;
        border-radius: 10px;
        display: grid;
        place-items: center;
    }
    button:hover{
        background-color: #ffff;
        transition: linear 2s ;
        cursor: pointer;
    }
    .error{
        color: #ffffff;
        font-size: 0.8rem;
    }
    
    .options{
        padding: 14px 0;
        overflow: hidden;
        font-size: 0.8rem;
        display: flex;
        flex-direction: column;
        gap:4px;
       
    }
    .options > p {
        position: relative;
        text-align: center;
        width: fit-content;
        margin: 0 auto;
        padding: 0 8px;
    }

    .options >p::after,
    .options >p::before{
        position: absolute;
        content: "";
        top: 50%;
        transform: translateY(-50%);
        width: 100vw;
        height: 1.5px;
        background:#C5D8D7;


    }
    .options > p::after{
        right: 100%;
    }
    .options > p::before{
        left: 100%;
    }
    .options div{
        display: flex;
        align-items: center;
        gap: 8px;
        justify-content: center;
    }
    .options div p:last-of-type{
        color: skyblue;
        cursor: pointer;
    }

    .spin{
        animation: spin 2s infinite;
    }
    @keyframes spin {
        from{
            transform: rotate(0deg);
        }
        to{
            transform: rotate(360deg);
        }
    }

</style>