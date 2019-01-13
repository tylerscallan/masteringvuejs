---
description: >-
  In this section we're going to start building up our app and we're going to
  create components and routes maintain the list of Suppliers that our company
  (Northwind Traders) has.
---

# Components: Suppliers

### Creating list component

We're going to create now a new folder under **views** called **Suppliers** and this will contain any view related to suppliers, in our case what we're looking to build is the list and edit views. So let's start by creating a new **SupplierList.vue** file which is not going to contain much to start with, just a header.

{% code-tabs %}
{% code-tabs-item title="SupplierList.vue" %}
```markup
<template>
    <div>
        <h1>Suppliers</h1>
    </div>
</template>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Creating a route

We're going to update the **router.js** to include this view as a route.

{% code-tabs %}
{% code-tabs-item title="router.js" %}
```javascript
/*...*/
{
  path: "/suppliers",
  name: "suppliers",
  component: () => import("./views/Suppliers/SupplierList.vue")
}
/*...*/
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Updating navigation

In the previous section you might have noticed that in the **App.vue** file there are a few router links, so we're going to update it to also include the link to the suppliers route just created

{% code-tabs %}
{% code-tabs-item title="App.vue" %}
```markup
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <!--Just created-->
      <router-link to="/suppliers">Suppliers</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Now if you navigate to **/suppliers**, that's what you're going to see the below which is not much, but a very good start.

![](../.gitbook/assets/suppliers-step-0.jpg)

### Displaying list of suppliers

Okay, back to the **SupplierList.vue**, we're going to make some meaningful changes so we can actually show a list of suppliers. We're going to start by creating a suppliers property in our data.

{% code-tabs %}
{% code-tabs-item title="SupplierList.vue" %}
```markup
<script>
export default {
  data() {
    return {
      suppliers: [
        {
          id: 4,
          companyName: "Tokyo Traders",
          contactName: "Yoshi Nagase",
          contactTitle: "Marketing Manager",
          address: {
            street: "9-8 Sekimai Musashino-shi",
            city: "Tokyo",
            region: "NULL",
            postalCode: 100,
            country: "Japan",
            phone: "(03) 3555-5011"
          }
        },
        {
          id: 5,
          companyName: "Cooperativa de Quesos 'Las Cabras'",
          contactName: "Antonio del Valle Saavedra",
          contactTitle: "Export Administrator",
          address: {
            street: "Calle del Rosal 4",
            city: "Oviedo",
            region: "Asturias",
            postalCode: 33007,
            country: "Spain",
            phone: "(98) 598 76 54"
          }
        }
      ]
    };
  }
};
</script>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

We're going to then update our template so we can display it. It doesn't look awesome yet because we're going to cover styling a little bit later. Also the **edit** link is not going to work right now as we haven't create the edit view yet, that's what we're doing next

{% code-tabs %}
{% code-tabs-item title="SupplierList.vue" %}
```markup
<template>
  <div>
    <h1>Suppliers</h1>
    <table>
      <thead>
        <tr>
          <th>Company</th>
          <th>Contact</th>
          <th>Title</th>
          <th>City</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="sup in suppliers" :key="sup.id">
          <td>{{sup.companyName}}</td>
          <td>{{sup.contactName}}</td>
          <td>{{sup.contactTitle}}</td>
          <td>{{sup.address.city}}</td>
          <td>
            <router-link :to="{name:'suppliers-edit',params:{id:sup.id, supplier:sup}}">Edit</router-link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

![That&apos;s how it should be looking at this stage](../.gitbook/assets/suppliers-step-1.jpg)

### Creating edit route

The main differences to the other route are first we're going to pass an **id** in the path, second we need to set **props** to true otherwise the component won't be able to read the data coming from the query string.

{% code-tabs %}
{% code-tabs-item title="router.js" %}
```javascript
{
  path: "/suppliers/:id",
  name: "suppliers-edit",
  component: () => import("./views/Suppliers/SupplierEdit.vue"),
  props: true
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Creating edit component

You should know the drill at this point, so simply create a **SupplierEdit.vue** and let's update it. In the script section, the main thing we need to set is the **props** property to allow us receiving the data. In this case we'll be able to get both the **id** or the whole **supplier** model.

{% code-tabs %}
{% code-tabs-item title="SupplierEdit.vue" %}
```markup
<script>
export default {
  props: {
    id: String | Number,
    supplier: Object
  }
};
</script>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

The template at this point will be super simple, just to display the data.

{% code-tabs %}
{% code-tabs-item title="SupplierEdit.vue" %}
```markup
<template>
  <div>
    <h1>{{id?`Supplier #${id}`:'New Supplier'}}</h1>
    <form>
      <div>
        <label>Company Name</label>
        <input type="text" v-model="supplier.companyName">
      </div>
      <div>
        <label>Contact Name</label>
        <input type="text" v-model="supplier.contactName">
      </div>
      <div>
        <label>Contact Title</label>
        <input type="text" v-model="supplier.contactTitle">
      </div>
    </form>
    <p>
      <router-link to="/suppliers">Cancel</router-link>
    </p>
  </div>
</template>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

In the next section we're going to update this component so we can update a supplier, but for that we're going to introduce the concept of service and api calls.
