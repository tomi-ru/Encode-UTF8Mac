# NAME

Encode::UTF8Mac - "utf-8-mac" encoding, a variant utf-8 used by Mac OSX

# SYNOPSIS

    use Encode;
    use Encode::UTF8Mac;
    

    my ($filename) = <*.txt>;

    $filename = Encode::decode('utf-8', $filename);
    # => "poke\x{0301}mon.txt" (NFD é)

    $filename = Encode::decode('utf-8-mac', $filename);
    # => "pok\x{00E9}mon.txt" (NFC é)

# DESCRIPTION

Encode::UTF8Mac provides a encoding called "utf-8-mac" for Mac OSX.

On Mac OSX, utf-8 encoding is used and it is NFD (Normalization Form
canonical Decomposition). If you want to get NFC (Normalization Form
canonical Composition) character you need to use [Unicode::Normalize](http://search.cpan.org/perldoc?Unicode::Normalize)'s
`NFC()`.

However, Mac OSX filesystem does not follow the exact specification.
Specifically, the following ranges are not decomposed.

    U+2000-U+2FFF
    U+F900-U+FAFF
    U+2F800-U+2FAFF

[http://developer.apple.com/library/mac/\#qa/qa2001/qa1173.html](http://developer.apple.com/library/mac/\#qa/qa2001/qa1173.html)

In iconv (bundled Mac), this encoding can be using as "utf-8-mac".
This module adds "utf-8-mac" encoding for [Encode](http://search.cpan.org/perldoc?Encode), it encode/decode text
with that rule in mind. This will help when you decode file name on Mac.

See more specific example:
[http://perl-users.jp/articles/advent-calendar/2010/english/24](http://perl-users.jp/articles/advent-calendar/2010/english/24)

# ENCODING

- utf-8-mac
    - Encode::decode('utf-8-mac', $octets)

        Decode as utf-8, and normalize form C except special range
        using Unicode::Normalize.

    - Encode::encode('utf-8-mac', $string)

        Normalize form D except special range using Unicode::Normalize,
        and encode as utf-8.

        OSX file system change NFD automatically. So actually, this is not necessary.

# COOKBOOK

    use Encode;
    use Encode::Locale;
    

    # change locale_fs "utf-8" to "utf-8-mac"
    if ($^O eq 'darwin') {
        require Encode::UTF8Mac;
        $Encode::Locale::ENCODING_LOCALE_FS = 'utf-8-mac';
    }
    

    $filename = Encode::decode('locale_fs', $filename);

If you are using [Encode::Locale](http://search.cpan.org/perldoc?Encode::Locale), you may want to do this.

# SEE ALSO

[Encode::Locale](http://search.cpan.org/perldoc?Encode::Locale) - provides usefull "magic" encoding.

[Unicode::Normalize::Mac](http://search.cpan.org/perldoc?Unicode::Normalize::Mac) - contains "utf-8-mac" logic.

# AUTHOR

Naoki Tomita <tomita@cpan.org>

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.
