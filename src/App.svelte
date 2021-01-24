<script>
  export let name;
</script>


<h1>Hello {name}!</h1>

<div>
  <p>SASS is working!</p>
</div>


<style type="text/scss">
  $color: red;

  h1 {
    color: $color;
    padding: 20px;
  }

  div {
    background: green;
    width: 90%;
    padding: 15px;
    margin: 10px auto;

    > p {
      color: #fff;
      font-weight: 800;
      font-size: 1em;
      text-align: center;
    }
  }
</style>

