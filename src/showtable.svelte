<script>
    import {createClient} from '@supabase/supabase-js'
    const supaurl = 'https://aigrlgsdtaxhicbspfqs.supabase.co';
    const supaapi = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImFpZ3JsZ3NkdGF4aGljYnNwZnFzIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MTcyMTc4NjksImV4cCI6MjAzMjc5Mzg2OX0.9JfpozFSgXpu5_uMN06oK5-Oq6uD2NYclcluDtJFvL8';
    const supabase = createClient(supaurl, supaapi)
    let users = []

    async function show(){
        const { data,error} = await supabase.from('users').select('*')
        if(error){
            console.log('error : ',error)
        }
        else{
            
        }
    }

</script>
<main>

</main>
<style>

</style>