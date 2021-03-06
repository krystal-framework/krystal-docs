Text component
=============

This component has several tools to work with text.

# Slug generator

    \Krystal\Text\SlugGenerator

It can be used to generate URL slugs. It has only one method called `generate()`. Let's start from a basic example:

    use Krystal\Text\SlugGenerator;
    
    $string = 'Hello World!';
    
    $sg = new SlugGenerator();
    echo $sg->generate($string); // hello-world

If you work with strings that aren't English and want not to romanize them, you can pass `false` to the constructor.

# Text trimmer

    \Krystal\Text\TextTrimmer

You probably have already seen how blogging sites trim article contents on their sites. This class is meant to do the trimming, Let's start from a basic example:

    <?php
    
    use Krystal\Text\TextTrimmer;
    
    $content = '..Here is a string with a very long content..';
    $length = 20; // Maximal length
    
    $tt = TextTrimmer();
    echo $tt->trim($content, $length);

This will trim that long string, appending `...` at the end. If you want to change a text which is appended by default, you can pass an optional argument to the constructor, which is an override.
