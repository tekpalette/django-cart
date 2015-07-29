**NOTE: django-cart is a different project starting on March 25th, 2009. Thanks a lot to Eric Woudenberg for letting us reuse this project**

django-cart is a very simple application that just let you add and remove items from a session based cart. django-cart doesn't provide any product information, and you've to define your own product models, and then use their instances on the cart.

A basic usage of django-cart could be:

_views.py_
```
from cart import Cart
from myproducts.models import Product

def add_to_cart(request, product_id, quantity):
    product = Product.objects.get(id=product_id)
    cart = Cart(request)
    cart.add(product, product.unit_price, quantity)

def remove_from_cart(request, product_id):
    product = Product.objects.get(id=product_id)
    cart = Cart(request)
    cart.remove(product)

def get_cart(request):
    return render_to_response('cart.html', dict(cart=Cart(request)))
```

_templates/cart.html_
```
{% extends 'base.html' %}

{% block body %}
    <table>
        <tr>
            <th>Product</th>
            <th>Quantity</th>
            <th>Total Price</th>
        </tr>
        {% for item in cart %}
        <tr>
            <td>{{ item.product.name }}</td>
            <td>{{ item.quantity }}</td>
            <td>{{ item.total_price }}</td>
        </tr>
        {% endfor %}
    </table>
{% endblock %}
```