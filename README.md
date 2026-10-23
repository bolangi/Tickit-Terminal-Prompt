# Tickit::Terminal::Prompt

A small terminal interface with a prompt and scrolling output.
The entry field has key bindings modeled after readline.

The application supplies `on_enter` and interprets the command text itself.

With a small amount of additional code this library can be
used to build a shell-like command processor for terminal
applications.

## Install

Requires Perl 5.36 or later, IO::Async, Tickit::Async,
Tickit::Widget::Entry, and Tickit::Widget::Scroller.

```sh
perl Makefile.PL
make
make test
make install
```

Try the sample application from this directory:

```sh
perl -Ilib examples/demo.pl
```

## API

```perl
my $ui = Tickit::Terminal::Prompt->new(
    prompt => 'demo> ',
    on_enter => sub ($ui, $line) {
        $ui->say("You entered: $line");
        $ui->stop if $line eq 'quit';
    },
);
$ui->run;
```

`print` appends text to the scroller; `say` appends text and a newline.
`set_prompt` changes the prompt while preserving the current editable text.
`stop` ends the event loop and returns control to the shell.

The prompt is part of the Entry text but is protected from Backspace, Left,
Home, Ctrl-A, Ctrl-U, and Ctrl-W. Ctrl-K deletes from the cursor to the end;
Ctrl-U deletes from the start of the editable command to the cursor. Ctrl-C
stops the interface. These bindings are internal in this first version.

## License

Artistic License 2.0; see `LICENSE`.
