# Blurry Html with transform: translate()

I had just finished creating some modal dialogs today and was just getting our UX designer to sign off on them when he pointed out that one of them was a bit blurry. I had sort of noticed this before, but didn't really think too much of it, attributing it to my monitor or not wearing my glasses. When I looked into it further, it was quiet obvious:

![Blurry dialog](../assets/blurry-dialog.png)
![Crisp dialog](../assets/crisp-dialog.png)

## The issue

After digging around online I found an explanation. The problem was caused by how the modal was being centred:
`transform: translate(0, 50%)`. The height of this particular modal was an odd number of pixels, which, when divided by 2 (50%) in the translate function, returned a result with a remainder of 0.5 pixels. E.g.: 33px / 2 = 16.5px.

While this seems fairly innocuous, Chrome (or more particularly Webkit) tries to render the half-pixel, causing a blurred outline around text.

## The solution

There are a few different ways to resolve this, which might work for you:
>Note: If you don't want to override your `transform: translate...`, you can pass it multiple arguments such as `transform: translate(0, 50%) scale(2);`

### Round the height of your element

Use the following lines of CSS to round the height of your element to an even number.

```scss
    transform: scale(2);
    zoom: 0.5;
```

### Enable subpixel antialising

Supposedly Chrome supports subpixel antialising which, in theory, should resolve this issue. However this particular style had no impact when I tried, but others seem to have luck with it. The css for that is `-webkit-font-smoothing: subpixel-antialiased;`

### Give your div a fixed (even) height

This was the best solution for me, as the modal component we were using was designed to shrink and grow based on the content and screen size. By changing this to work within a fixed size, I was able to ensure that the dialog would render correctly regardless of the device it was viewed on.
