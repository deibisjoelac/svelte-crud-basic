<script>
    import { onMount } from "svelte";
    let product = {};
    let search = '';
    let tempProducts = [];
    let products = [
        {
            id: 1,
            name : 'Magitech Laptop Toshiba Satellite C45-A4116FL',
            description : 'Intel Core i5-4200U 1.6GHz, Pantalla LED 14" HD, 4 GB RAM / Disco Duro 500GB, DVD±RW, Bluetooth 4.0, Windows 8.1',
            url : 'https://www.magitech.pe/media/catalog/product/cache/1/image/650x/040ec09b1e35df139433887a97daa66f/e/r/erter.jpg',
            price : 720
        },
        {
            id: 2,
            name : 'Laptop HP Pavilón',
            description : '15-CS0073 INTEL CORE I7 1 TB 16GB',
            url : 'https://ovalo24.com.pe/wp-content/uploads/2019/11/1622-600x600.jpg',
            price : 750
        }
    ];
    tempProducts = products
    const initForm = () => {
        product = {
            id: "",
            name: "",
            description: "",
            url: "",
            price: ""
        };
    };

    const saveUpdateProduct = e => {
        if(product.id) {
            const indexItem = products.findIndex(pro => pro.id === product.id);
            products[indexItem] = product;
        }
        else {
            product.id = products.length + 1;
            products = [...products, product];
        }
        tempProducts   = products
        initForm();
    };
    const deleteProduct = (id) => {
        products = products.filter(el=> el.id !== id);
        tempProducts   = products
    };
   const  setProductEdit = (pro)=> {
      product = pro;
    }
    const searchProduct = ()=>{
        
        if( search){
            products = products.filter(el=> 
                el.name.toLowerCase().includes(search.toLowerCase())
                ||  el.description.toLowerCase().includes(search.toLowerCase()));
        }else{
            products = tempProducts
        }
      
    }
    onMount(() => {
        initForm();
    });
</script>

<style>
main{
    font-size: 0.85rem !important;
}
</style>

<main>

  <div class="container-fluid p-5">
    <div class="row">
      <div class="col-xl-7">
            <div class="row mt-2 mb-2">
                <div class="col-xl-4  mx-auto my-auto">
                    <h5 class="">List Products</h5>
                </div>
                <div class="col-xl-8">
                    <input class="form-control" bind:value={search} on:input="{searchProduct}"
                         placeholder="Search for Name or Description" />
                </div>
            </div>
          
            {#each products as pro}
                <div class="card mb-2" >
                  <div class="row no-gutters">
                        <div class="col-md-4">
                            {#if pro.url}
                                <img src="{pro.url}" class="card-img" alt="{pro.name}" />
                            {:else}
                                <img src="img/noproduct.png" class="card-img" alt="{pro.name}" />
                            {/if}
                        </div>
                        <div class="col-md-8">
                            <div class="card-body">
                                <h5 class="card-title">{pro.name}</h5>
                                <p class="card-text">{ pro.description}</p>
                                <p class="card-text">  $ { pro.price} </p>
                                <button class="btn btn-info btn-sm" on:click="{setProductEdit(pro)}">Editar</button>
                                <button class="btn btn-danger btn-sm" on:click="{deleteProduct(pro.id)}">Delete</button>
                            </div>
                        </div>
                    </div>
                </div>
            {:else}
                <div class="card">
                    <div class="card-body p-3">
                        <p>Not products filter result</p>
                    </div>
                </div>
                
            {/each}
        

      </div>
      <div class="col-xl-5">
        <div class="card">
            <div class="card-header">
                Form Product
            </div>
            <div class="card-body">
                <form on:submit|preventDefault={saveUpdateProduct} autocomplete="off">
                    <input type="hidden" name="{product.id}">
                    <div class="form-group">
                        <label for="">Name Product</label>
                        <input class="form-control" bind:value={product.name} />
                    </div>
                    <div class="form-group">
                        <label for="">Description Product</label>
                        <textarea  class="form-control" bind:value={product.description} rows="3"></textarea>
                
                    </div>
                    <div class="form-group">
                        <label for="">URL Image Product</label>
                        <input class="form-control" bind:value={product.url} />
                    </div>
                    <div class="form-group">
                        <label for="">Price Product $</label>
                        <input class="form-control" bind:value={product.price} />
                    </div>

                    <button class="btn btn-success">Save</button>
                    <button class="btn btn-secondary" type="button" on:click="{initForm}">Clear</button>
                </form>
            </div>
        </div>
      </div>
    </div>
  </div>
</main>
