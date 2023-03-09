*commentline.txt*  Commentline Operator

Author:  Shiyue He <hsy1995313@gmail.com>
License: Same terms as Vim (see |license|)

A lightweight comment operator based on 'vim9script'.

<Leader>c{motion}                               <Leader>c
                        Smart comment or uncomment 

<Leader>cc                                      <Leader>cc
                        Comment lines

<Leader>cu                                      <Leader>cu
                        Uncomment lines

[l, r] -> commentstring_without_space
Comment -> indent_line -> do_comment
Uncomment -> indent_line -> do_uncomment
Smartcomment -> is_comment -> Comment / Uncomment

 vim:tw=78:et:ft=help:norl:
