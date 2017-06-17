# Weird TMUX edge case 2017-06-17

## Issue Summary
While writing a C program that delt with encryption, I needed to view some encrypted files determine their contents. I used ``cat`` to do so, and occasionally got some weird results.

0. The Problem:
![Tmux character encoding][tmux_edge_case.png]

Printing one file would change the character encoding of the terminal, while another would change it back to the original.

1. I determined that there was a single character responsible in the file the binary file for changing the character encoding by reducing the file until ``cat file.txt`` only printed out single character in question.

2. To confirm my tests, I wrote a simple C program that would print every ASCII character to a file, and deleted characters from the file until the my suspicions were confirmed.

## Result
The character in question is ASCII code 0x0E (^N), which turns the character encoding into an emoji style characterset, but only in tmux. The ASCII character 0x0F (^O) will turn the characterset back to the default character set.

If the encoding changes in tmux, I will print the character 0x0F (^O).
