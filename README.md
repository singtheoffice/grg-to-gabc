# GRG-to-GABC

This script converts GRG files in bulk to their equivalent GABC files.

To convert a batch of GRG files, add them in an `input` folder adjacent to the converter script:

    converter.py
    lookup-table.txt
    input/
     - a.grg
     - b.grg
     - c.grg

Running `converter.py` generates an `output` folder containing the GABC files.

    output/
     - a.gabc
     - b.gabc
     - c.gabc

`lookup-table.txt` is required to specify the correct GABC notation for any neum that the GRG file contains. For example, `lookup-table.txt` could be as follows:

    1   s   ;
    2   s   ,
    3   s   ::
    12  n   ji
    19  n   ef
    24  n   i
    161 x

and then upon running `converter.py`, you may get a message alerting you that you need to provide GABC notation for lookup code 20:

    $ python converter.py
    Processing some-file.grg
    - Needs lookup for code 20
    - Wrote output/some-file.gabc

When you look inside `some-file.gabc` you will find `<<<LOOKUP 20>>>` in the location where the script needs to know the notation.

This allows you to update the lookup table to contain the correct GABC notation for code 20 (based on your knowledge of what the output should look like):

    1   s   ;
    2   s   ,
    3   s   ::
    12  n   ji
    19  n   ef
    20  n   hj
    24  n   i
    105 x
    161 x

The notation of the lookup file is that each line has the lookup code followed by some whitespace, followed by an "s" if it's a special character sequence, like the dividers, "n" if it's notes, and "x" if that lookup code should be ignored. Note that the converter is smart and only needs eg. ji provided in the lookup table to be able to fill in ba, fe, and so on when code 20 is provided at different pitches in the GRG file. Also, the entries don't have to be in numerical order.

Now when you run `converter.py` again, it should completely convert the file.

Please consider contributing your updated `lookup-table.txt` back to the project so that others can save time when converting their own files.
