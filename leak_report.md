# Leak report

There were 46 bytes lost, they were allocated by calloc, which was called on
line 41 of check_whitespace.c in the function strip, on line 62 of
check_whitespace in the function is_clean, and on line 87 of check_whitespace.c
in the function main.

These memory errors happen because the function strip allocates memory every
time it is called, but the memory is never cleared after it is done being used
which causes bytes to be lost.

The errors can be fixed by freeing the allocated memory after it is done being
used. In is_clean, cleaned needs to be cleared before a result is returned
because cleaned is pointing to the allocated memory. However we only want to
free clear if it is not an empty string, that would be freeing memory that was
never allocated.

