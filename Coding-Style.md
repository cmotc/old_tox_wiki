 
###Chapter 1: Indentation
 
Indentation is _4 spaces_.
 
The preferred way to ease multiple indentation levels in a switch statement is
to align the "switch" and its subordinate "case" labels in the same column
instead of "double-indenting" the "case" labels.  E.g.:
 
    switch (suffix) {
    case 'G':
    case 'g':
        mem <<= 30;
        break;
    case 'M':
    case 'm':
        mem <<= 20;
        break;
    case 'K':
    case 'k':
        mem <<= 10;
        /* fall through */
    default:
        break;
    }
 
 
Don't put multiple statements on a single line unless you have
something to hide:
 
    if (condition) do_this;
        do_something_everytime;
 
Don't put multiple assignments on a single line either. Avoid tricky expressions.

Get a decent editor and _don't leave whitespace at the end of lines_.
 

###Chapter 2: Breaking long lines and strings
 
Coding style is all about readability and maintainability using commonly
available tools.
 
The limit on the length of lines is 120 columns and this is a strongly
preferred limit.
 
Statements longer than 120 columns will be broken into sensible chunks, unless
exceeding 120 columns significantly increases readability and does not hide
information. Descendants are always substantially shorter than the parent and
are placed substantially to the right. The same applies to function headers
with a long argument list. However, never break user-visible strings such as
printk messages, because that breaks the ability to grep for them.
 
 
###Chapter 3: Placing Braces and Spaces
 
The other issue that always comes up in C styling is the placement of
braces.  Unlike the indent size, there are few technical reasons to
choose one placement strategy over the other, but the preferred way, as
shown to us by the prophets Kernighan and Ritchie, is to put the opening
brace last on the line, and put the closing brace first, thusly:
 
    if (x is true) {
        we do y
    }
 
This applies to all non-function statement blocks (if, switch, for,
while, do).  E.g.:
 
    switch (action) {
    case KOBJ_ADD:
        return "add";
    case KOBJ_REMOVE:
        return "remove";
    case KOBJ_CHANGE:
        return "change";
    default:
        return NULL;
    }
 
However, there is one special case, namely functions: they have the
opening brace at the beginning of the next line, thus:
 
    int function(int x)
    {
        body of function
    }
 
Heretic people all over the world have claimed that this inconsistency
is ...  well ...  inconsistent, but all right-thinking people know that
(a) K&R are _right_ and (b) K&R are right.  Besides, functions are
special anyway (you can't nest them in C).
 
Note that the closing brace is empty on a line of its own, _except_ in
the cases where it is followed by a continuation of the same statement,
i.e. a "while" in a do-statement or an "else" in an if-statement, like
this:
 
    do {
        body of do-loop
    } while (condition);
 
and
 
    if (x == y) {
        ..
    } else if (x > y) {
        ...
    } else {
        ....
    }
 
Rationale: K&R.
 
Also, note that this brace-placement also minimizes the number of empty
(or almost empty) lines, without any loss of readability.  Thus, as the
supply of new-lines on your screen is not a renewable resource (think
25-line terminal screens here), you have more empty lines to put
comments on.
 
Do not unnecessarily use braces where a single statement will do.
 
    if (condition)
        action();
 
and
 
    if (condition)
        do_this();
    else
        do_that();
 
This does not apply if only one branch of a conditional statement is a single
statement; in the latter case use braces in both branches:
 
    if (condition) {
        do_this();
        do_that();
    } else {
        otherwise();
    }
 
3.1:  Spaces
 
Use of spaces depends (mostly) on function-versus-keyword usage. Use a space after (most) keywords.  The
notable exceptions are sizeof, typeof, alignof, and \_\_attribute\_\_, which look somewhat like functions (and are usually used with parentheses, although they are not required in the language, as in: "sizeof info" after "struct fileinfo info;" is declared).
 
So use a space after these keywords:
    if, switch, case, for, do, while
but not with sizeof, typeof, alignof, or \_\_attribute\_\_.  E.g.,
    s = sizeof(struct file);
 
Do not add spaces around (inside) parenthesized expressions.  This example is
*bad*:
 
    s = sizeof( struct file );
 
When declaring pointer data or a function that returns a pointer type, the
preferred use of '*' is adjacent to the data name or function name and not
adjacent to the type name.  Examples:
 
    char *linux_banner;
    unsigned long long memparse(char *ptr, char **retptr);
    char *match_strdup(substring_t *s);
 
Use one space around (on each side of) most binary and ternary operators,
such as any of these:
 
    =  +  -  <  >  *  /  %  |  &  ^  <=  >=  ==  !=  ?  :
 
but no space after unary operators:
    &  *  +  -  ~  !  sizeof  typeof  alignof  \_\_attribute\_\_  defined
 
no space before the postfix increment & decrement unary operators:
    ++  --
 
no space after the prefix increment & decrement unary operators:
    ++  --
 
and no space around the '.' and "->" structure member operators.
 
Do not leave trailing whitespace at the ends of lines.  Some editors with
"smart" indentation will insert whitespace at the beginning of new lines as
appropriate, so you can start typing the next line of code right away.
However, some such editors do not remove the whitespace if you end up not
putting a line of code there, such as if you leave a blank line.  As a result,
you end up with lines containing trailing whitespace.
 
Git will warn you about patches that introduce trailing whitespace, and can
optionally strip the trailing whitespace for you; however, if applying a series
of patches, this may make later patches in the series fail by changing their
context lines.
 
 
###Chapter 4: Naming
 
C is a Spartan language, and so should your naming be.  Unlike Modula-2
and Pascal programmers, C programmers do not use cute names like
ThisVariableIsATemporaryCounter.  A C programmer would call that
variable "tmp", which is much easier to write, and not the least more
difficult to understand.
 
HOWEVER, while mixed-case names are frowned upon, descriptive names for
global variables are a must.  To call a global function "foo" is a
shooting offense.
 
GLOBAL variables (to be used only if you _really_ need them) need to
have descriptive names, as do global functions.  If you have a function
that counts the number of active users, you should call that
"count_active_users()" or similar, you should _not_ call it "cntusr()".
 
Encoding the type of a function into the name (so-called Hungarian
notation) is brain damaged - the compiler knows the types anyway and can
check those, and it only confuses the programmer.  No wonder MicroSoft
makes buggy programs.
 
LOCAL variable names should be short, and to the point.  If you have
some random integer loop counter, it should probably be called "i".
Calling it "loop_counter" is non-productive, if there is no chance of it
being mis-understood.  Similarly, "tmp" can be just about any type of
variable that is used to hold a temporary value.
 
If you are afraid to mix up your local variable names, you have another
problem, which is called the function-growth-hormone-imbalance syndrome.
See Chapter 6 (Functions).
 
 
###Chapter 5: Typedefs
 
Please don't use things like "vps_t".
 
It's a _mistake_ to use typedef for structures and pointers. When you see a
 
    vps_t a;
 
in the source, what does it mean?
 
In contrast, if it says
 
    struct virtual_container *a;
 
you can actually tell what "a" is.
 
Lots of people think that typedefs "help readability". Not so. They are
useful only for:
 
 (a) totally opaque objects (where the typedef is actively used to _hide_
     what the object is).
 
     Example: "pte_t" etc. opaque objects that you can only access using
     the proper accessor functions.
 
     NOTE! Opaqueness and "accessor functions" are not good in themselves.
     The reason we have them for things like pte_t etc. is that there
     really is absolutely _zero_ portably accessible information there.
 
 (b) Clear integer types, where the abstraction _helps_ avoid confusion
     whether it is "int" or "long".
 
     u8/u16/u32 are perfectly fine typedefs, although they fit into
     category (d) better than here.
 
     NOTE! Again - there needs to be a _reason_ for this. If something is
     "unsigned long", then there's no reason to do
 
    typedef unsigned long myflags_t;
 
     but if there is a clear reason for why it under certain circumstances
     might be an "unsigned int" and under other configurations might be
     "unsigned long", then by all means go ahead and use a typedef.
 
 (c) when you use sparse to literally create a _new_ type for
     type-checking.
 
 (d) New types which are identical to standard C99 types, in certain
     exceptional circumstances.
 
     Although it would only take a short amount of time for the eyes and
     brain to become accustomed to the standard types like 'uint32_t',
     some people object to their use anyway.
 
     Therefore, the Linux-specific 'u8/u16/u32/u64' types and their
     signed equivalents which are identical to standard types are
     permitted -- although they are not mandatory in new code of your
     own.
 
     When editing existing code which already uses one or the other set
     of types, you should conform to the existing choices in that code.
 
 (e) Types safe for use in userspace.
 
    In certain structures which are visible to userspace, we canno     t
    require C99 types and cannot use the 'u32' form above. Thus,      we
    use __u32 and similar types in all structures which are s     hared
    with userspace.
  
    Maybe there are other cases too, but the rule should basically be to NEVER
    EVER use a typedef unless you can clearly match one of those rules.
  
    In general, a pointer, or a struct that has elements that can reasonably
    be directly accessed should _never_ be a typedef.
 
 
###Chapter 6: Functions
 
Functions should be short and sweet, and do just one thing.  They should
fit on one or two screenfuls of text (the ISO/ANSI screen size is 80x24,
as we all know), and do one thing and do that well.
 
The maximum length of a function is inversely proportional to the
complexity and indentation level of that function.  So, if you have a
conceptually simple function that is just one long (but simple)
case-statement, where you have to do lots of small things for a lot of
different cases, it's OK to have a longer function.
 
However, if you have a complex function, and you suspect that a
less-than-gifted first-year high-school student might not even
understand what the function is all about, you should adhere to the
maximum limits all the more closely.  Use helper functions with
descriptive names (you can ask the compiler to in-line them if you think
it's performance-critical, and it will probably do a better job of it
than you would have done).
 
Another measure of the function is the number of local variables.  They
shouldn't exceed 5-10, or you're doing something wrong.  Re-think the
function, and split it into smaller pieces.  A human brain can
generally easily keep track of about 7 different things, anything more
and it gets confused.  You know you're brilliant, but maybe you'd like
to understand what you did 2 weeks from now.
 
In source files, separate functions with one blank line.  If the function is
exported, the EXPORT* macro for it should follow immediately after the closing
function brace line.  E.g.:
 
    int system_is_up(void)
    {
        return system_state == SYSTEM_RUNNING;
    }
    EXPORT_SYMBOL(system_is_up);
 
In function prototypes, include parameter names with their data types.
Although this is not required by the C language, it is preferred because 
it is a simple way to add valuable information for the reader.
 
  
###Chapter 7: Commenting
 
Comments are good, but there is also a danger of over-commenting.  NEVER
try to explain HOW your code works in a comment: it's much better to
write the code so that the _working_ is obvious, and it's a waste of
time to explain badly written code.
 
Generally, you want your comments to tell WHAT your code does, not HOW.
Also, try to avoid putting comments inside a function body: if the
function is so complex that you need to separately comment parts of it,
you should probably go back to ###Chapter 6 for a while.  You can make
small comments to note or warn about something particularly clever (or
ugly), but try to avoid excess.  Instead, put the comments at the head
of the function, telling people what it does, and possibly WHY it does
it.
 
The style for comments is the C89 "/* ... */" style.
Don't use C99-style "// ..." comments.
 
The preferred style for long (multi-line) comments is:
 
    /*
     * This is the preferred style for multi-line
     * comments in the Project Tox source code.
     * Please use it consistently.
     *
     * Description:  A column of asterisks on the left side,
     * with beginning and ending almost-blank lines.
     */
 
It's also important to comment data, whether they are basic types or derived
types.  To this end, use just one data declaration per line (no commas for
multiple data declarations).  This leaves you room for a small comment on each
item, explaining its use.
 
###Chapter 8: Data structures
 
Data structures that have visibility outside the single-threaded
environment they are created and destroyed in should always have
reference counts.  Garbage collection is slow and inefficient, which
means that you absolutely _have_ to reference count all your uses.
 
Reference counting means that you can avoid locking, and allows multiple
users to have access to the data structure in parallel - and not having
to worry about the structure suddenly going away from under them just
because they slept or did something else for a while.
 
Note that locking is _not_ a replacement for reference counting.
Locking is used to keep data structures coherent, while reference
counting is a memory management technique.  Usually both are needed, and
they are not to be confused with each other.
 
Many data structures can indeed have two levels of reference counting,
when there are users of different "classes".  The subclass count counts
the number of subclass users, and decrements the global count just once
when the subclass count goes to zero.
 
Examples of this kind of "multi-level-reference-counting" can be found in
memory management ("struct mm_struct": mm_users and mm_count), and in
filesystem code ("struct super_block": s_count and s_active).
 
Remember: if another thread can find your data structure, and you don't
have a reference count on it, you almost certainly have a bug.
 
 
###Chapter 9: Macros, Enums and RTL
 
Names of macros defining constants and labels in enums are capitalized.
 
`#define CONSTANT 0x12345`
 
Enums are preferred when defining several related constants.
 
CAPITALIZED macro names are appreciated but macros resembling functions
may be named in lower case.
 
Generally, inline functions are preferable to macros resembling functions.
 
Macros with multiple statements should be enclosed in a do - while block:
 
`#define macrofun(a, b, c)`
              
    do {                    
        if (a == 5)             
            do_this(b, c);      
    } while (0)``
 
Things to avoid when using macros:
 
1) macros that affect control flow:
 
`#define FOO(x)`                
  
    do {                    
        if (blah(x) < 0)        
            return -EBUGGERED;      
    } while(0)
 
is a _very_ bad idea.  It looks like a function call but exits the "calling"
function; don't break the internal parsers of those who will read the code.
 
2) macros that depend on having a local variable with a magic name:
 
`#define FOO(val) bar(index, val)`
 
might look like a good thing, but it's confusing as hell when one reads the
code and it's prone to breakage from seemingly innocent changes.
 
3) macros with arguments that are used as l-values: FOO(x) = y; will
bite you if somebody e.g. turns FOO into an inline function.
 
4) forgetting about precedence: macros defining constants using expressions
must enclose the expression in parentheses. Beware of similar issues with
macros using parameters.
 
`#define CONSTANT 0x4000`

`#define CONSTEXP (CONSTANT | 3)`
  
###Chapter 10: Allocating memory
 
The kernel provides the following general purpose memory allocators:
kmalloc(), kzalloc(), kmalloc_array(), kcalloc(), vmalloc(), and
vzalloc().
 
The preferred form for passing a size of a struct is the following:
 
    p = kmalloc(sizeof(*p), ...);
 
The alternative form where struct name is spelled out hurts readability and
introduces an opportunity for a bug when the pointer variable type is changed
but the corresponding sizeof that is passed to a memory allocator is not.
 
Casting the return value which is a void pointer is redundant. The conversion
from void pointer to any other pointer type is guaranteed by the C programming
language.
 
The preferred form for allocating an array is the following:
 
    p = kmalloc_array(n, sizeof(...), ...);
 
The preferred form for allocating a zeroed array is the following:
 
    p = kcalloc(n, sizeof(...), ...);
 
Both forms check for overflow on the allocation size n * sizeof(...),
and return NULL if that occurred.
 
 
###Chapter 11: The inline disease
 
There appears to be a common misperception that gcc has a magic "make me
faster" speedup option called "inline". While the use of inlines can be
appropriate (for example as a means of replacing macros, see ###Chapter 12), it
very often is not. Abundant use of the inline keyword leads to a much bigger
kernel, which in turn slows the system as a whole down, due to a bigger
icache footprint for the CPU and simply because there is less memory
available for the pagecache. Just think about it; a pagecache miss causes a
disk seek, which easily takes 5 milliseconds. There are a LOT of cpu cycles
that can go into these 5 milliseconds.
 
A reasonable rule of thumb is to not put inline at functions that have more
than 3 lines of code in them. An exception to this rule are the cases where
a parameter is known to be a compiletime constant, and as a result of this
constantness you *know* the compiler will be able to optimize most of your
function away at compile time. For a good example of this later case, see
the kmalloc() inline function.
 
Often people argue that adding inline to functions that are static and used
only once is always a win since there is no space tradeoff. While this is
technically correct, gcc is capable of inlining these automatically without
help, and the maintenance issue of removing the inline when a second user
appears outweighs the potential value of the hint that tells gcc to do
something it would have done anyway.
 
 
###Chapter 12: Function return values and names
 
Functions can return values of many different kinds, and one of the
most common is a value indicating whether the function succeeded or
failed.  Such a value can be represented as an error-code integer
(-Exxx = failure, 0 = success) or a "succeeded" boolean (0 = failure,
non-zero = success).
 
Mixing up these two sorts of representations is a fertile source of
difficult-to-find bugs.  If the C language included a strong distinction
between integers and booleans then the compiler would find these mistakes
for us... but it doesn't.  To help prevent such bugs, always follow this
convention:
 
If the name of a function is an action or an imperative command,
the function should return an error-code integer.  If the name
is a predicate, the function should return a "succeeded" boolean.
 
For example, "add work" is a command, and the add_work() function returns 0
for success or -EBUSY for failure.  In the same way, "PCI device present" is
a predicate, and the pci_dev_present() function returns 1 if it succeeds in
finding a matching device or 0 if it doesn't.
 
All EXPORTed functions must respect this convention, and so should all
public functions.  Private (static) functions need not, but it is
recommended that they do.
 
Functions whose return value is the actual result of a computation, rather
than an indication of whether the computation succeeded, are not subject to
this rule.  Generally they indicate failure by returning some out-of-range
result.  Typical examples would be functions that return pointers; they use
NULL or the ERR_PTR mechanism to report failure.