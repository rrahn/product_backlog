# Gather issues for the Product Backlog from the old projects

## Modules

As a [role] I want [feature] so that [reason]
[action] the [result] [by|for|of|to] a(n) [object]

### Build system

### C++20 and pre-SeqAn 3.1 release

* Update tutorials and wiki
* Module dependencies?
* Compile times for alignment should be reevaluated (impact on API?)
* Compile times for I/O should be reevaluated (impact on API?)
* Compile times for search should be reevaluated (impact on API?)
* binning directories and minimisers and gapped shapes
* zstd compression
* lzma compression
* memory compressed sequences
* cram support
* I want to be able to jump to a region inside of the annotation file using an index for bcf
* To read/write structural variant information we need to have annotation I/O Fileformats VCF, BCF, GTF, GFF< Variant information structural vairants
* It should be possible to jump inside of a sequence file with an fasta index
* I want to be able to jump to an  alignment region inside of the alignment file using an index for the bam files
* SeqAn2 release
* Googletest should track master Cereal should be stable
* Constraint: The CI build process should finish within 10 minutes. Using modules could dramatically reduce compile times for snippets for example
* As a developer I want to have more scalable build resources such that CI can run through much faster and I don't have to wait to long for these results.
* Search performance should not be slower than 10% of the original SeqAn2 code on the most current compiler with respect to memory multiplied with runtime.
* Alignment performance should not be slower than 10% of the original SeqAn2 code on the most current compiler such that user can expect same runtimes for their applications
* Sequence I/O performance should not be slower than 10% of the original SeqAn2 code on the most current compiler such that user can expect same runtimes for their applications
* Sam/Bam I/O performance should not be slower than 10% of the original SeqAn2 code on the most current compiler such that user can expect same runtimes for their applications
* TODO: Reevaluate the ranges module and the interfaces of them once c++20 has been finally released.
* Constraint: We need a stable Cereal release, or we need an alternative.
cereal stable?
    - evaluate until 24 Dec 2019 -> If not reevaluate the situation with cereal
    - Reevaluate cereal: https://github.com/USCiLab/cereal/issues/563
    Likely we have to make a full fork or switch to a different project. Possibilities:
    Alternatives:
        - yas
        - cista++
        - See also https://github.com/thekvs/cpp-serializers
* Constraint: We need a stable SDSL release.
    - [ ] EPR dictionary?
    - [ ] who maintains the SDSL
    - [ ] can we add another maintainer of the SeqAn team
* Constraint: c++20 has to be released before we can fix our implementation on the current standard.

#### Compiler support

* gcc-11 support
* <span style="color:orange">Clang support https://github.com/seqan/seqan3/issues/404</span> -> Do all interfaces work properly with concepts?
* Add Support for Visual Studio 2019 version 16.3 which now has concepts support!
* add compiler macros to detect compiler and version

#### Test/CI

* Wishlist for GitHub actions https://github.com/seqan/product_backlog/issues/198
    * Use -D_GLIBCXX_ASSERTIONS in our CI builds https://github.com/seqan/product_backlog/issues/231
    * preview docs for each pull request https://github.com/seqan/seqan3/issues/166
    * travis without zlib bzip support https://github.com/seqan/seqan3/issues/1182
    * nightlies should be built with seqan2 in benchmarks! What about CI?
    * ~~Enable memory tests in jenkins https://github.com/seqan/seqan3/pull/278~~
    * Extensive unit tests not run by default https://github.com/seqan/seqan3/issues/511

* **snippets**
    * Snippets should be tested for their stdout: https://github.com/seqan/seqan3/issues/416
    * In **snippet builds**, please remove  -Werror=unused-parameter. In Snippets it is very often desired to just declare or initialise variables without actually using them.
    * the code snippets that produce an output as used by the tutorials should be tested against their output. This also means that the output can be included in the tutorial and thus is changed when the snippet changes.

* **static code analysis**
    * Add support for clang-tidy
    * setup automatic styleguide checking https://github.com/seqan/seqan3/issues/168
    * https://scan.coverity.com/faq#what-is-coverity-scan Can be integrated into travis - maybe as nightly?
    * Setup linter. - [ ] Which linter's are available?

* **code coverage**
    * More honest code coverage with templated code: https://github.com/emilydolson/force-cover
    * code coverage: increase total coverage to at least 99% for all modules.
    * Currently the code coverage seems to have problems with `if constexpr` statements.
    The same is true for `constexpr` expressions in general.
    Maybe this is related to some gcc bug which needs to be investigated.
    * Currently the code coverage tests all paths that can throw or not throw. This is sometimes not possible since throwig might depend on system propoerties. One could explicitly annotate the unhittable code paths with LCOV macros.
    Exclude unhittable code paths:
    https://stackoverflow.com/questions/27623733/exclusion-markers-for-gcov/30078276#30078276

* **unit tests**
    * Underscores in test and test suite names https://github.com/seqan/product_backlog/issues/76
    * modularize tests *per modelling concept one test template*
        * for example `fm_index`, `bi_fm_index` https://github.com/seqan/seqan3/blob/c7aa6a711c0b0d6a8bc8eff4a62034403eeb512c/test/unit/search/fm_index_test.cpp
    * Add random tests / fuzzing
    * Change build system to support detail tests that are excluded from coverage. https://github.com/seqan/seqan3/issues/1166
    * Building/running tests without internet connection: https://github.com/seqan/product_backlog/issues/223
    * As a external package manager I want to run the SeqAn3 library test suite with an installed gtest version so that I can manage the dependencies during installation of the package.
        _Acceptance criteria_
        * Use FetchContent in Cmake to get the gtest version at configure time which should also expose the correct variables for gtest and gtest_main (see discussion: #1627)
        * The user can opt-in using installed system gtest library if wanted
    * As a downstream packager that runs the library tests before packaging the sources for the package manger I want to know the minimum gtest version required such that I can make sure that if I use the packaged gtest library it has the minimum required version.
        _acceptance tests_
        - documentation page for how to use system installed gtest instead of fetched gtest is added in the online documentation;
  * the cmake test script checks for the minimum gtest version and errors with a diagnostic message if it is not fulfilled.
    * Fix cached variables with cmake config. Currently if e.g. the compiler does not work or a dependency is not found, you cannot re-configure without deleting `CMake*`.
    * [follow-up#1094] Test that `seqan3-config.cmake` works with cmake 3.4. https://github.com/seqan/product_backlog/issues/140
    * cmake: test whether each all.hpp includes all non-detail header files in the current dir. https://github.com/seqan/seqan3/issues/568
    * re-evaluate whether header tests can be split into individual tests and named more descriptively.
    remove this includes https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/alphabet_test_template.hpp#L11-L13 and replace them by forwards

* **benchmark**
    * [DOC, TEST] Consistent use of `benchmark::DoNotOptimize` https://github.com/seqan/product_backlog/issues/214
    * Is it possible to have the benchmark html reports generated automatically.
    Can they be compared to previous benchmarks to detect performance regression? Phoronix suite?
    Could we embed reports into reference manuals?
    * Warn in cmake if performance tests are build in debug mode  https://github.com/seqan/seqan3/issues/450

#### Platform/Packaging
* Add `-Wconversion`
    * -Wconversion https://github.com/seqan/seqan3/issues/896
* Compatibility with LTO? https://github.com/seqan/seqan3/issues/2013
* autotools in pc.in
* Re-evaluate the whole process of shipping app binaries to older platforms, including the question of linkage and pthread.
* Add Suport for Meson Build system
* ~Add Support for BioConda package management~
* Add Support for Conan Package management

### Documentation

#### Infrastructure

* Provide .gitpod.yml to try out seqan3 in an online editor https://github.com/seqan/product_backlog/issues/137
* [Doxygen] Improve API search https://github.com/seqan/seqan3/issues/468
    See http://www.doxygen.nl/manual/searching.html
    1. Is what we are currently doing.
    2. Looks like [this](https://docs.seqan.de/seqan/develop_server_based_search/search.php?query=index) but you have to enter your search term and press enter (no live search).
    3. Combines 1.+2. + some extras. But requires us to be allowed to run cgi on the server (ask IT).
* Write wiki entry for HowTo write Doxygen: https://github.com/seqan/seqan3/issues/132
* python comparison -> seqan 3.1 paper
* detect dead links in documentation: https://github.com/seqan/seqan3/issues/232
* wiki: we should add guidelines for concept naming and other namings

#### API
* Document the way our header structure looks like (module/library structure)
* Document why we use (or not use) `all.hpp` headers in the tutorials (API documentation).
* prefix all sub group identifier (names) with the group identifier (e.g. subgroup structure in group alphabet should be alphabet_structure)
* documentation for `mask` contains `to_char` and `assign_char` because inherited from base. This is wrong and confusing because they aren't actually inherited, because `mask` is only semialphabet.
* documentation for `mask` contains `to_char` and `assign_char` because inherited from base. This is wrong and confusing because they aren't actually inherited, because `mask` is only semialphabet.

#### Cookbook

* Add sequence conversion to cookbook https://github.com/seqan/product_backlog/issues/215

#### Tutorials
* [DOC] cmake default without -O3 and order of args/options in cmake call: https://github.com/seqan/seqan3/issues/2017
* revisit setup guide: https://github.com/seqan/product_backlog/issues/139
* In the setup tutorial there is a list of files and folders of the seqan3 directory. I suggest to append a slash to the directory names in order to distinguish them from the files, i.e.
    ```
    tutorial/
    ├── source/
    ├── build/
    └── seqan3/
        ├── build_system/
        ├── doc/
        ├── include/
        ├── submodules/
        ├── test/
        ├── CHANGELOG.md
        ├── CMakeLists.txt
        ├── CODE_OF_CONDUCT.md
        ├── CONTRIBUTING.md
        ├── LICENSE.md
        └── README.md
    ```
* <span style="color:orange">use `std::iter_difference_t<it_t>;` everywhere and document this in the wiki</span>
* <span style="color:orange">Read mapper tutorial is needlessly complicated, why are sequences not serialised together with index?</span>
* <span style="color:orange">Index Tutorial and Read Mapper Tutorial use std::span in weird way, they should just use `views::slice`.</spab>
* Improve Tutorial - Implement your own read mapper
    - Introduction, Agenda, The data - 3 min
    - The indexer
        - Step 1 - Parsing arguments - 28 min
        - Step 2 - Reading the input - 20 min
        - Step 3 - Index - 12 min
    - The read mapper
        - Step 1 - Parsing arguments - 19 min
        - Step 2 - Reading the input and searching - 20 min
        - Step 3 - Alignment - 30 min
        - Step 4 - Alignment output - 13 min
    ---
    sum: 145 min
* Improve Tutorial - Alignment Input and Output
    - Detailed time consumption (based on a one person run through):
        - Introduction, Alignment file formats, Alignment file fields - 5 min
        - Reading alignment files - 6+16 min
        - Alignment representation in the SAM format, Completing reference information - 8+45 min
             Reading the CIGAR string - 2 min
        - Writing alignment files - 2+6
    ---
    Sum: 90 min

    - File extensions Snippet prints [sam]
        seqan3::debug_stream << seqan3::format_sam::file_extensions << std::endl; // prints [fastq,fq]
    - Construction Snippet does not stop
    - Reading custom fields Snippet throws an error:
        terminate called after throwing an instance of 'seqan3::format_error'
          what():  [CORRUPTED SAM FILE] The string 'VN:1.6' could not be cast into type short unsigned int
    - Same header problem for the following Exercise: Accumulating mapping qualities.
* Improve Tutorial - Index and Search:
    - Detailed time consumption (based on a one person run through):
        - Introduction (Capabilities, Terminology) - 4
              - Example - 7+20
        - Search (Terminology) - 1
              - Searching for exact matches - 14
              - Searching for approximate matches - 17
              - Search modes - 12
              - One last exercise - 40
      ---
      Sum: 115 min

        -  In this chapter it is very noticeable that the tutorial was written by different people. Because here    overview   chapter Capability and Terminology are used. Maybe one should introduce this uniformly in the other     chapters  aswell.
        - Assignment 5: The whole task took a long time, as much knowledge is needed from the alignment section. Maybe  one  should specify as an hint how the align-config looks like.

* Improve Tutorial - Pairwise Alignment:
    - Detailed time consumption (based on a one person run through):
        - Introduction - 3
        - Computing pairwise alignments - 14
        - Alignment configurations - 3
            - Global and semi-global alignment - 14
            - Scoring schemes and gap schemes - 18
            - Alignment result - 16
            - Banded alignment - 7
            - Edit distance - 16
            - Invalid configurations - 1
    ---
    Sum: 92 min
    - Text correction needed: "SeqAn currently supports currently ..."
    - Assignment 5 - How does the result change? In my solution it stays the same.
* Improve Tutorial - Sequence File Input and Output:
    - Detailed time consumption (based on a one person run through):
    - File I/O in SeqAn (Basic layout of SeqAn file objects) - 4
    - Sequence file formats - 3
    - Fields - 1
    - Reading a sequence file
         - Construction (File traits) - 4
         - Reading records - 9
         - The record type - 8
    - Sequence files as views
         - Reading a file in chunks, Applying a filter to a file - 4
         - Reading paired-end reads - 16
    - Writing a sequence file
         - Construction - 2
         - Writing records - 18
         - Files as views - 5
    - File conversion - 1
    ---
    Sum: 75 min
    - Error message: 'accumulate' is not a member of 'ranges' (Sequence files as views -> Applying a filter to a file) change ranges::accumulate to std::accumulate and include <numeric>.

* Improve Tutorial - Ranges:
    - Detailed time consumption (based on a one person run through):
    - Motivation - 2 min
    - Ranges (Range concepts, Storage behaviour) - 6 min
    - Views
        -  Lazy-evaluation - 2 min
        -  Combinability - 25 min
        - View concepts - 7 min
        - Views in the standard library and in SeqAn - 20 min
    - Containers (The bitcompressed vector) -25 min
    ---
    Sum: 85 min
    - Small grammar correction: "... used as flexibly as iterators ..." must be 'flexible'
* Improve Tutorial - Alphabets:
   - Introduction - 5
   - The nucleotide alphabets (Construction and assignment of alphabet symbols, The rank of an alphabet symbol, The char representation of an alphabet symbol) - 3 min
       - Obtaining the alphabet size, containers over alphabets, Comparability - 3 min
       - Example - 30 min
   - Other alphabets (The amino acid alphabet, Structure and quality alphabets, Gap alphabet) - 3 min
  ---
  Sum: 45 min
* Improve Tutorial - C++ Concepts:
    - Constraints (Motivation - 8, Syntax variants - 2) - 10 min
    - Terminology - 3 min
    - Overloading and specialisation (Function (template) overloading - 25, Partial template specialisation - 2) - 27 min
    - Concepts in SeqAn and this documentation - 5 min
    - How to make your own type model a concept seqan3::validator - 25 + x min
    ---
    Sum: 70 + x min
    - Overloading and specialisation -> Partial template specialisation: Description to short to be understandable.
    - How to make your own type model a concept: Exercise to complex to do it without the solution & "It should fail for the arguments -i 3; and/or -j 144 or -j 3." fail only after the second exercise.
    - Grammar and sentence structure must be revised (some examples: "most often", "in so far", "said to model said", "inherited from.")
* Improve Tutorial - Parsing command line arguments with SeqAn:
    - Assignment 3
    "Set the default value of aggregate_by to "mean"" -> (ist bereits gesetzt)
    - game of parsing: Beispieldatei besser csv als  anstatt tsv, da manche Editoren /t mit Leerzeichen ersetzen.
    -> Error message: Could not cast '' to a valid number.
    - Chaining validators
    Full solution wirft Error message: Segmentation fault: 11
        Das Problem scheint am seqan3::regex_validator zu liegen.
* Write serialisation page: https://github.com/seqan/seqan3/issues/187
* Manual writing with `\page`s in doxygen framework See http://www.doxygen.nl/manual/commands.html#cmdpage for `\page` command
* Discuss the use of "symbol" in the Alphabet Tutorial. Maybe letter instead of sybmbol?

### Alignment

#### General
* <span style="color:red">Concepts for sequence and alignment: https://github.com/seqan/product_backlog/issues/60</span>
* What are the lifetime guarantees of align_pairwise and should we document them?
    ```c++
    sequence_pair;
    auto alignment;

    {
    auto alignment_range = align_pairwise(sequence_pair); // lvalue and compute alignment

    alignment = alignment_range[0];
    } // alignment has no dangling reference as long as sequence_pair exist


    {
    auto alignment_range = align_pairwise(std::move(sequence_pair)); // rvalue and compute alignment

    alignment = alignment_range[0]; // alignment is non-dangling as long as alignment_range exists
    } // alignment is dangling, because alignment_range is out-of-scope
    ```

    same scenario, but without computing the alignment, `alignment` variable is never dangling.

    this get more complicated, because in some cases the `gap_decorator` isn't used as an aligned sequence and in   these cases we copy the unaligned_sequence.

    related: https://github.com/seqan/seqan3/pull/1618
* <span style="color:red">A complete new way to call alignment_pairwise.</span>
    Changes to `align_pairwise`  discussed on 01.07.2019:

    ```
    input: collection -> n x n
    input: collection, collection -> n x m
    input:collection<pair> -> 1x1
    ```

    During the iterator planning we discussed how to incorporate these interfaces but we have another suggestion:

    ```
    using sequence_pair = std::tuple<sequence_t, sequence_t>;

    align_pairwise([sequence_pair, ...]); // current syntax

    align_pairwise([(sequence_pair, id), ...]); // new syntax
    ```

    ---

    h-2 (updated 20200116)
    I completey agree with @marehr now. There should be only one interface of `collection<tuple>`. The tuple can be either `(seq1, seq2)` or `(seq1, seq2, id1, id2)` so there won't be too much nesting.
    `views::combine_pairwise` should be adapted, see my card in the range-canban.
* Cleanup of the alignment module:
    renamings:
    row_index_type -> row_index
    column_index_type -> col_index
    trace_directions -> trace_direction

    structural:
    remove alignment_coordinate in favor of generic matrix_index and expose only single positions in the alignment result class not pairs. The pairs are not useful anyway
    add alignment error_code to alignment result.
    Also don't call it back_coordinate in the result options but end position and begin position. Then we do not worry about the coordinate meaning anymore

    Update the documentation of it.
    * Currently we have a matrix_index and two aliases for this class template called matrix_coordinate and matrix_offset. The matrix_coordinate could be replaced by instances of the matrix_index as the meaning is really the same. Moreover we have an alingment_coordinate which we expose to the user in the result of the alignment. This should be either an alias of the matrix_index or not exposed to the user. The user might only want to know the end/begin position of an alignment within the respective sequences. The association to a coordinate within the matrix is not relevant for the use afterwards.
    * Clean up the all headers. A lot of detail stuff is included by the all headers which should only include public stuff. Clean up alignment module

* Gap decorator optimisations
    * Gap decorator anchor block: https://github.com/seqan/seqan3/pull/917
    * Gap Decorator Variants Benchmarked: https://github.com/seqan/seqan3/issues/382
    * Gap Decorator with bulk insertion of gaps and/or a read-only decorator https://github.com/seqan/seqan3/issues/1080
    * `seqan3::gap_decorator` should use a sorted `std::vector` instead of a `std::set`, because it's much faster! The only theoretical advantage of the set is log-inserts, but we need to update the tail anyways so our inserts are always linear.
    * `seqan3::gap_decorator` currently has O(log(k)) back-insertion of gaps but could have O(1). Just check if `it == end()` and directly access last element of anchor set without doing upper_bound.

* Scoring scheme optimisations
    * add `.set_ednafull()` member function on nucleotide_scoring_scheme that sets this scoring matrix.
    * <span style="color:red">requirement for `::score_type` should be remove from `scoring_scheme` concept. The score type is whatever is returned by the score function!</span>
    * <span style="color:red">Rename scoring scheme enums (e.g BLOSSUM) to lower case</span>

* Alignment landing page should show code example per Headline  https://github.com/seqan/product_backlog/issues/138
* Should we have a cigar_vector class wrapping std::vector<seqan3::cigar>? This allows to put some of the very often used conversion functions to put them as part of the interface opposed to the not so generic free functions that we have at the moment.

#### Pairwise alignment acceleration

* Vectorised alignment - dynamic vectorisation support https://github.com/seqan/product_backlog/issues/115
* Add vectorised alignment computation of the traceback https://github.com/seqan/seqan3/issues/1572
* Add parallelisation support for the alignment module: https://github.com/seqan/product_backlog/issues/57
    * Currently we store the scoring scheme as a state in the alignment algorithm. In parallel mode the entire algorithm has to be copied every time it is put on the queue. Investigate if this affects the runtime in parallel execution?
* Add basic vectorisation support for the dna global affine alignment: https://github.com/seqan/product_backlog/issues/54
* Extend pairwise alignment interface to distinguish different user inputs https://github.com/seqan/product_backlog/issues/211
* Vectorised alignment with protein sequences: https://github.com/seqan/product_backlog/issues/206
* Simd profile score https://github.com/seqan/product_backlog/issues/210
* Macrobenchmark for protein sequence alignment: https://github.com/seqan/product_backlog/issues/208
* [Meta-Issue] Alignment Module https://github.com/seqan/seqan3/issues/314 -> revisit and maybe delete

* Improved Edit Distance: https://github.com/seqan/product_backlog/issues/117
* Use edit distance fixtures in all edit distance related test cases https://github.com/seqan/product_backlog/issues/128
* Implement the banded myers https://github.com/seqan/seqan3/issues/890
    Currently blocked by this feature: reopen  #1207

#### Multiple sequence alignment

* Position Specific Score Matrix: https://github.com/seqan/product_backlog/issues/102
* MSA and Alignment Graph: https://github.com/seqan/seqan3/issues/1763
* PR https://github.com/seqan/seqan3/pull/1548 has some comments that should be incorporated in a following cleanup.

### Alphabet

* uint64_t is not an alphabet: https://github.com/seqan/product_backlog/issues/200
* [PERF BUG] improve perfomance of dna4::complement: https://github.com/seqan/seqan3/issues/1970
* <span style='color'>Should translation tables be a run-time choice, too? Right now it is only compile_time; benchmark compile-time influence? https://github.com/seqan/seqan3/issues/111
* phred alphabet naming:
    phred63 seems arbritrary because the phred scale is actually of size 94. So it should either be phred94 or maybe just phred?
    phred42 was chosen to be able to use a dna5-phred42 alphabet_tuple that still fits in one byte. This could be named small_phred.
* An alphabet can satisfy `seqan3::detail::constexpr_semialphabet` (it only requires `to_rank` to be constexpr), but might not be comparable in constexpr context, like in this case:
    ```c++
    struct semialphabet_shim
    {
        using rank_t = uint8_t;

        constexpr rank_t to_rank() const noexcept;

        constexpr static bool alphabet_size{0};

        friend bool operator<(semialphabet_shim, semialphabet_shim);
        friend bool operator<=(semialphabet_shim, semialphabet_shim);
        friend bool operator>(semialphabet_shim, semialphabet_shim);
        friend bool operator>=(semialphabet_shim, semialphabet_shim);
        friend bool operator==(semialphabet_shim, semialphabet_shim);
        friend bool operator!=(semialphabet_shim, semialphabet_shim);
    };
    ```
* Rename translation frames enum to lower case
* nucleotide_base converting constructor is under-constrained. Should require constexpr_writable_alphabet and have non-constexpr fallback.
* `convert_through_char_representation` is underconstrained, should require constexpr_writable_alphabet.
* `convert_through_char_representation` has SeqAn2-style wrong order of parameters (first out than in).
* Discuss the use of "symbol" in the Alphabet Tutorial. Maybe letter instead of sybmbol?
* implement `alphabet_any`
* make union_composition require the same char_type on alternatives
* https://github.com/seqan/seqan3/issues/111
* alphabet test
    remove `#include <.../all.hpp>` dependencies in `*_test_template.hpp` and fix broken test cases afterwards (due to missing includes). For example

    https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/nucleotide/nucleotide_test_template.hpp#L13

    should not include the `seqan3/alphabet/nucleotide/all.hpp` header and every test which includes `nucleotide_test_template` should add his own type as `#include <.../alphabet/.../own_type.hpp>`.
* alphabet test
    remove `#include <.../all.hpp>` calls and replace them with the minimal needed includes, like in
    https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/mask/mask_test.cpp#L11
    and
    https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/nucleotide/nucleotide_conversion_integration_test.cpp#L12

### Argument Parser
* [Argument Parser] Remove restriction for `-` in subparser app name https://github.com/seqan/product_backlog/issues/234
* Smaller wording errors:
    https://github.com/seqan/seqan3/pull/1666#discussion_r398211518
    https://github.com/seqan/seqan3/pull/1666#discussion_r398216242
    https://github.com/seqan/seqan3/pull/1666#discussion_r398216356
* exceptions should be converted to error_codes instead. This makes future optimisations on handling exceptions by the language much easier -> SeqAn 4?
* Rename option_spec enum names to lower case
* the value parameter to `add_option` is marked as an out-parameter, but it should be in/out as the current value is used to generate the default, right?
* --license to print copyright https://github.com/seqan/seqan3/issues/862
* Tuebingen should use https. Bug them if you see this!
* Proposal: Split out seqan3/argument_parser into its own project https://github.com/seqan/seqan3/issues/1272
* Refactor detail function `retrieve_value` to return the value type.
* Required options should be marked as such. `e.g. with Option is required.`
* Add `arithmetic_min/max` validators
* We should make the argument parser ping our own server not Tuebingen
* export html format: Would be nicer if we could use rapidxml somehow to test the DOM tree.
* Issue with implicit conversion when creating file_ext_validator with two char pointers (`file_ext_validator tmp{{"sam", "bam"}};`)
* When one long_id is the prefix of another long_id, and the first one is added first, calling the second long_id will not work.
* Should the validator types be semiregular?
* A lot of string parameters are passed by reference and are then copied, instead of copied in the function call and then moved (copy elision)
* Write more (complex, integration) tests
* parsing should be configurable by a config file (reopen  https://github.com/seqan/seqan3/issues/193)
* Proposal: Split out seqan3/argument_parser into its own project https://github.com/seqan/seqan3/issues/1272
* Argument parser - export of file format (ctd) for KNIME engine: https://github.com/seqan/seqan3/issues/89
* [Argument Parser] Enable modification of default value display: https://github.com/seqan/seqan3/issues/1744
* To extract the argument parser we need to cut loose of SeqAn3 dependencies https://github.com/seqan/product_backlog/issues/40
* [Argument Parser] Follow up: Document POSIX conventions that are implemented in the argument parser: https://github.com/seqan/product_backlog/issues/201
* ArgParser: extension containing . accepted, but fail: https://github.com/seqan/seqan3/issues/1176
* Argument parser wrong copyright year and missing metadata: https://github.com/seqan/seqan3/issues/2173
* Sequence file input validator does not accept gzipped files: https://github.com/seqan/seqan3/issues/1591

### Core
* Core / Utility Split: https://github.com/seqan/product_backlog/issues/160
* Design proposal for executors in alignment and search: https://github.com/seqan/product_backlog/issues/80
* [DOC] Write landing page for Core/Concept https://github.com/seqan/product_backlog/issues/155
* [DOC] Fix the landing page for Core/Simd https://github.com/seqan/product_backlog/issues/154
* Get `seqan3::debug_stream` straight once and for all: https://github.com/seqan/product_backlog/issues/63
* Introduce seqan3/std/bit and deprecate most of seqan3/core/bit_manipulation.hpp https://github.com/seqan/product_backlog/issues/199
* Consider using https://github.com/nemequ/simde/ for cross CPU SIMD https://github.com/seqan/seqan3/issues/1391
* Make call of get in seqan3::configuration consistent https://github.com/seqan/product_backlog/issues/195
* type_list_specialisation is a weird name.
    before the great concept rename it was called `TypeList`, so snake casing it would have been a name conflict. Are there other concept names that had this conflict?

    ---

    https://gist.github.com/bad-ed/72d7d1310b988ffd8ea6dc2bd21ffcda

    ---

    Results of discussing it on 2019-11-11, @h-2:

    * For checking whether something `T` is a specialisation of `foo<>` we often need a shortcut that is defined as `is_type_specialisation_of_v<T, foo>` or semantically equivalent.
    * The name `foo_specialisation` is clear, but not beautiful.
    * If we change it, we should follow concept naming guidelines, i.e. it should be a noun.
    * Independent of this we might want a concept for `type_specialisation_of` for the generic case. But depending on the resolution of certain NB comments, concepts with incomplete types may become ill-formed.
* We need to enable versioned cerealisation https://uscilab.github.io/cereal/serialization_functions.html#explicit-versioning
* add_enum_bitwise_operators issues?
    - [ ] Remove the `add_enum_bitwise_operators` from the public API documentation using `//cond DEV`
    - [ ] Document the bitwise functionality in each enum that uses  `add_enum_bitwise_operators`
    - [ ] Pull `add_enum_bitwise_operators` into the detail namespace such that it works for enums that are declared    in the detail namespace
    - [ ] Do the same for strong types!
* cereal concepts should all go into detail as they are not relevant for users. The cereal archives should also get aliases in `seqan3::`.
* printing by debug_stream should have tests, e.g. printing alignments was broken and no-one realised it.
* the range returned by algorithms should be a (move-only) view.
* ~~`seqan3::is_in_alphabet<>` (char operations) can be removed, because we have `seqan3::char_is_valid_for<>` (alphabet module)~~
* move debug_stream_alphabet into `alphabet/concept.hpp`, because core should not depend on alphabet.
* get on seqan3::record should be a CPO
    - [ ] Remove all free get functions in record.hpp
    - [ ] Remove std overloads tuple_size and tuple_element in `record.hpp`
    - [ ] Introduce a get CPO that looks for
    - [ ] member .get()
    - [ ] custom struct
    - [ ] adl get
    - [ ] std::get
    Note(hannes): there is no ADL get. NOTE (zoRRo) We
* Module  dependencies, Hannes Proposal:
    We should establish these rules:
    * modules may not depend on each other, with the following exceptions:
        * all except std and contrib may depend on core and detail
        * alignment, search and i/o may depend on alphabet and range but not on each other
* Strong types lack serialisation support https://github.com/seqan/seqan3/issues/240
* Always use std::error_code for exceptions... https://github.com/seqan/seqan3/issues/273
* add [[likely]] and [[unlikely]] attributes https://github.com/seqan/seqan3/issues/275
* SIMD support: https://github.com/seqan/seqan3/issues/531
* Think about whether we want our own assert macro. It should not throw, though.
* debug asserts with stack trace and message https://github.com/foonathan/debug_assert
* Make all customization point objects into Niebloids
* add simd_literal allow
    ```c++
    simd_t simd = 5_simd;
    ```
* add simd algorithms, `min, max,`
* Put configuration stuff into a subdirectory within core/algorithm/configuration
* Consider using https://github.com/nemequ/simde/ for cross CPU SIMD


### I/O

* Initializing one sequence and one alignment file takes 47s and uses all 8 CPU cores: https://github.com/seqan/seqan3/issues/1195
* BAM unknown reference name https://github.com/seqan/seqan3/issues/1201
* I/O Performance: https://github.com/seqan/product_backlog/issues/130
* Improve FastQ I/O https://github.com/seqan/product_backlog/issues/129
* parse CIGAR std::string into std::vector<cigar> https://github.com/seqan/seqan3/issues/1231
* IO: Hardware Accelerated ZIP: https://github.com/seqan/product_backlog/issues/166
* [Sequence IO] BAM support  https://github.com/seqan/seqan3/issues/1418
* Implement CLUSTAL format for Alignment I/O: https://github.com/seqan/product_backlog/issues/147
* [Alignment IO] reading hexadecimal SAM tags part 1 https://github.com/seqan/seqan3/issues/1414
* [Alignment IO] reading hexadecimal SAM tags part 2 https://github.com/seqan/seqan3/issues/1416
* [IO] SAM/BAM performance: https://github.com/seqan/product_backlog/issues/227
* [IO] Possible improvements for the SAM/BAM format parsing: https://github.com/seqan/product_backlog/issues/226

* Should we use phred63 as a default quality alphabet since it accepts more values? Or maybe we could add a phred_legal_alphabet that is ohred63 and then the normal quality alphabet is still phred42
* Currently the bam io uses take_exactly_or_throw during the parsing extensively. However, using this functionality will cause different errors than the expected format_error, namely the unexpected_end_of_input. This is at some places inconsistent. For example if the quality string (<l_seq) is too small it might throw unexecpted_end_of_input, whereas if the quality string is too long (>l_seq) it might throw format_error.
Expected behavior would be that always format errors are thrown and the errors are consistent.
* `fields` should be variable template, not a class template.
    a) because `field` is a value and not a type
    b) because it makes `{}` necessary whenever its used
* Memory mapping: In SeqAn 2 we have the following tests (and app tests) that use memory mapping:
    105 - test_test_bam_io (Failed)
    124 - test_test_gff_io (Failed)
    136 - test_test_index_crosscompare_dna (Failed)
    165 - test_test_pipe (Failed)
    183 - test_test_seq_io (Failed)
    184 - test_test_sequence (Failed)
    185 - test_test_sequence_v2 (Failed)
    207 - test_test_stream (Failed)
    211 - test_ucsc_io (Failed)
    212 - test_test_vcf_io (Failed)
    509 - app_test_micro_razers (Failed)
    517 - app_test_razers (Failed)
    539 - app_test_yara (Failed)
    A candidate for a cross platform solution for memory mapping seems to be https://github.com/mandreyel/mio
* default width for lines in fasta should be 70 not 80.
* The field types in `sequence_file_output` never undergo any type checking, e.g. the seq field should at least be required to be an input range over an alphabet type.
* The documentation for I/O has become very unwieldy by making the snippets copy'n'pastable. Have a look at the first snippet of `sequence_file_input`! The snippet is only supposed to illustrate that you don't need to pass template arguments (one line!!). Instead there is 20+ lines of boilerplate that is very difficult to parse (the first snippet of sequence_file_input should not use sequence_file_output!).
Let's please use single-line snippets for these very simple things again. Users need to see that they can pass `"FOOBAR.fasta"` to the constructor and not `asdfasdf sdf sdfsdf; sdfsdfsdf; sequence_file_in{tmp_file/"BAR"}; asdfsdfsdfsdf`
* `output_stream_over` is a wrong name, because we are not checking if the second argument is the value type of the stream. It can be an arbitrary type as long as it can be streamable...
I am not sure we need these concepts at all. One should just not take constrained template parameters of stream, just take istream or ostream or whatever directly. It's all virtual in any case.
* `views::istreambuf` should be moved to `io/stream` from the ranges module.
* debug_stream and fmtflags2 still falsely appear as part of I/O module (they are part of core now)
* remove ostream_iterator and ostreambuf_iterator, use from std module instead.
* fields_specialisation should be moved to detail, it's irrelevant for users.
* What happens on PPC with bam tests when the byte order should be converted to big endian.
* Since we remove the file column interface, we need to implement a unzip function. The file column based interface would have great performance impact so we should re evaluate to use it. E.g. with concatenated sequences. We should implement a output_unzip function which is better than using views.
* For cigar conversion we want:
    - [ ] `views::to_char<cigar>`
    - [ ]  `views::char_to<cigar>
    - [ ]  free function to convert `std::vector<cigar>` to an alignment
    - [ ]  free function to convert an alignment to `std::vector<cigar>`
* **Formats**:
  - [x] vienna (part of #307)
  - [ ] stockholm 1
  - [ ] connect
  - [ ] stockholm 2 ?
    Here some links:
  * http://projects.binf.ku.dk/pgardner/bralibase/RNAformats.html
  * http://www.ibi.vu.nl/programs/k2nwww/static/data_formats.html
    The Stockholm 1 structured alignment format is important, because it is the common format in the Rfam and Pfam data bases.
* Formats that should be implemented
  - [x] FastA
  - [x] FastQ
  - [x] SAM (WIP by @MitraDarja )
  - [x] BAM
  - [x] embl
  - [x] genbank
  - [ ] Fast5
* Compressor Streams
    - [x] zlib_istream
    - [x] bzip_istream
    - [x] bgzf_istream
    - [ ] zstd_istream

* Decompressor Streams
    - [x] zlib_ostream
    - [x] bzip_ostream
    - [x] bgzf_ostream
    - [ ] zstd_ostream
* Other file formats support "GFT, BED, etc.." @team: reopen https://github.com/seqan/seqan3/issues/1163 when working on it
* Support Compressed FASTA indexing @team: reopen https://github.com/seqan/seqan3/issues/591 when working on it
* parse CIGAR std::string into std::vector<cigar> https://github.com/seqan/seqan3/issues/1231
* Initializing one sequence and one alignment file takes 47s and uses all 8 CPU cores https://github.com/seqan/seqan3/issues/1195
* Design change;

    I) The (stream, format) constructor of the files should actually be a (stream, variant<..>)-constructor so that choice of the format is still a run-time decision.
    This will make the interface much more consistent, there are virtually no drawbacks.

    II) Stream type should not be taken as template parameter either. Just take basic_istream.

    -> None of the constructors is now a template, we might be able to remove  all of the deduction guides.
* We want the files to be default constructible
* [Alignment IO] Read field::CIGAR into a std::string
    In the current code, you cannot change the type of the cigar vector. It is always `std::vector<cigar>`. This should be changed to also allow a std::string.

    - [ ] Make field::CIGAR type be configurable through the alignment file input traits
    - [ ] Change the format_sam and format_bam code to handle std::string types as input
    - [ ] Change the format_sam and format_bam code to handle std::string types as output
* BAM unknown reference name https://github.com/seqan/seqan3/issues/1201
* Better error messages for  IO
    * line numbers would be nice (which record was processed, for this we need a count variable for the format).
    * IDs and more information should be printed.
    * maybe introduce a to_string function
* Design Decision: Should File extension be case insensitive? (currently they are case sensistive)
* Re-evaluate if all the static asserts in the format read/write functions should go into the file.
* Change Type trait concept in file io to static asserts https://github.com/seqan/seqan3/issues/969
* Should we always do (expensive) checks for the SAM format (e.g. each record reference id must be present in the header ref_dict if header is given). Or maybe introduce a input/output option for this?
* Build our tests with
    ```cpp
    sync_with_stdio(false);
    ```

### Utility

* Handling of "bit-patterns"  is inconsistent. The `dynamic_bitset` takes string literals`"0101"` while the shape gets an extra strong type that is initialised with a numeric literal in binary notation: `bin_literal{0b10101}`.
    I suggest the following:
    1. rename `bin_literal` to `bits`
    2. give `bits` a constructor from number and one from string_view.
    3. introduce `0b101_bits` and `"101"_bits` as user defined literals of type `bits`.
    4. Use `bits` in the constructors of dynamic_bitset and shape.

### Ranges

* A Life Without range-v3 https://github.com/seqan/product_backlog/issues/124
* views::translate_join | views::deep{std::views::reverse} causes ICE https://github.com/seqan/seqan3/issues/1520
* Move container, sequence_container, ... into test https://github.com/seqan/product_backlog/issues/221
* iterator_test_template: add test for contiguous_iterator
* Expand Iterator test template to test contiguous iterators. https://github.com/seqan/product_backlog/issues/94
* UB in kmer_hash_view https://github.com/seqan/product_backlog/issues/165
* [DOC] Write landing page for Search/K-mer Index: https://github.com/seqan/product_backlog/issues/153
* Journal string: https://github.com/seqan/product_backlog/issues/111
* iterator_test_template: add sized_sentinel property https://github.com/seqan/product_backlog/issues/192
* Post Mortem of unexpected 2x speed-up in minimiser view https://github.com/seqan/product_backlog/issues/126
* iterator_test_template: add test for contiguous_iterator https://github.com/seqan/product_backlog/issues/193
* Refactor the container_test_template.hpp do not use dna4 explicitily: https://github.com/seqan/product_backlog/issues/150
* Replace the ranges::views::zip with our own implementation: https://github.com/seqan/product_backlog/issues/132
* Performance of minimiser view with second range https://github.com/seqan/product_backlog/issues/148
* std::common_with requirement in seqan3::complement CPO triggers ICE: https://github.com/seqan/product_backlog/issues/149

* Our pure input iterators (e.g. lazy result range in search) should become move-only once the iterator concepts do not require copy ability anymore.
* our containers like `small_vector` assume the value_type is a builtin-type in many places, e.g. value-semantics, no proper moves, elements are not destructed on `pop_back` or negative resize...
* https://github.com/seqan/seqan3/pull/856 was closed due to many open questions regarding the structure of this. In general, it would be interesting to know if this can help reducing tests for ranges. So we might reevaluate it later.
* some changes to `views::pairwise_combine`:
    * rename to `views::all_pairs`
    * make view factory instead of adaptor
    * have single param interface (like now)
    * add two-param interface where all pairs between the sets are generated
    * change the tuple-type to be `(seq1, seq2, id1, id2)` where id1 and id2 are numbers that indicate the original IDs of the sequences

    The latter enables the design proposed by @marehr for the align function.
* member of `async_input_view::iterator` is currently `mutable value_type`. This implicitly required default-constructibility and also copyability so long as that is required. Maybe better to wrap in semiregular_box wrapper.
* It seems there are some regressions between gcc compilers working on views:
    from https://github.com/seqan/seqan3/pull/1371
    ```
    On my machine these are the numbers for copy 1D on GCC7:

    copy<translate_tag>                   509 ns          509 ns      1314110
    copy<seqan::Serial>                   781 ns          781 ns       881368

    And the same benchmark on GCC9:

    copy<translate_tag>                   906 ns          906 ns       774028
    copy<seqan::Serial>                   726 ns          726 ns       960984

    There is some really funky stuff going on with GCC in regard to handling views. Unfortunately the trend is not in our favour.
    Someone ™️ should investigate this more thoroughly and report it to GCC.
    ```
* remove `.cbegin()` from our views because the standard library views don't have it and it's confusing.
* re-design views::translate_single to use a custom iterator like views::kmer_hash (instead of random_access_iterator_base) because of performance. (measure whether it actually improves performance before merging)
* move `views::istreambuf` to the IO module
* rename `reservible_container` to `reservable_container` (typo everywhere)
* all view concept tables should be annotated with whether they require/preserve std::semiregular. Currently all preserve this, but we need to test/annotate which also require it -> likely involves some fixing.
* gap_decorator's `assign_unaligned` should be a CPO. It's broken when being called qualified right now.
* //TODO: combining chunk and join is substantially faster than views::interleave (2.5x), why? see format_fasta.hpp
* our iterator_tag metafunction should not be necessary.
* Once the range library has no implicit conversion of views anymore, search the library for `GCC diagnostic ignored "-Wdeprecated-declarations"` and remove.
* Give up our own definitions in <concepts> once  ericniebler/range-v3#1322 is resolved.
* Reactivate test in view_interleave_test once ericniebler/range-v3#1320 ius merged.
* [FEATURE] add seqan3::view::join<> https://github.com/seqan/seqan3/pull/104
  implement views::join_with / what about stale join-PR?
* Hash of `std::vector<dna4>` is linear, should be constant for unordered_maps. https://github.com/seqan/seqan3/issues/1122
* AlignedRange and gap_decorator See https://github.com/seqan/seqan3/issues/1103
* Remove associated types from view classes and files classes https://github.com/seqan/seqan3/issues/914
* All views shall have explicit constructors https://github.com/seqan/seqan3/issues/941
* change behaviour of take/drop according to P1739/P1664
* improve documentation of source views / middle views / sink views
* move out ranges that are specific to other modules
* Hiwi work: Take out all the huge iterators from the parent class.
* Replace ranges::common_pair with our own implementation
* **Containers**: make a CRTP Mixin class that pre defines all those constructors and insert functions to delegate to a single insert function of the derived class that needs to be implemented. Also: remove the range constructor Also: Take a look at initializer list constructors: Can they be replaced by parameter packs?
* When iterating over the gap_decorator there is no way to check if the current position is a gap or not. Instead one has to dereference the value first and then check of this was a gap. But when reading an alignment file without reference sequence one cannot determine the gap blocks in the reference because dereferencing the value causes an exception due to an invalid access. Add functionality to the gap decorator and the the corresponding iterator to check if a given position is indeed a gap or something else.
* Fix random access iterator to a proper CRTP base class with private constructors.
* `view::take_line | std::view::reverse` still has problems. See the test and https://github.com/seqan/seqan3/pull/864#discussion_r272027494
* Should the view constructors be explicit, if they get only one argument?
* view::persist should downgrage ContiguousIterator/Range to RandomAccessIterator/Range because contiguous suggests you can directly access the memory whose lifetime might end, if not access via view::persist.
* Make view pairwise combine view not require common range
* Restructure the gap_decorator_anchor_set once views are required to be movable instead of copyable as this will cause incompatibility otherwise.
* aligned_sequence_concept should also require a clear_gap function
* **Bitcompressed Vector**  expose growth factor (depends on changes in SDSL)
* translate view from amino acid to nucleotides
* **view submodule**
    * [x] `complement`
    * [x] `convert`
    * [x] `get<i>`
    * [x]  `translate`
    * [ ] `unzip` (view-"sink")
    remove `alphabet/range.hpp`
* **container submodule**
    * [x] `concatenated_sequences`
    * [x] ` bitcompressed_vector`
    * [ ] `mmapped_vector`
    * [ ] `delta_compressed_vector`
* **Matching Statistics**
    * implementation of bidirectional matching statistics according to "Indexed matching statistics and shortest unique substrings" Belazzougui et al.
    * using a lot sdsl
* [FEATURE] Add zip view https://github.com/seqan/seqan3/pull/1264

### Search

* Port Lambda's search engine to use EPR dictionaries within SeqAn3/SDSL: https://github.com/seqan/product_backlog/issues/20
* Use a buffer to cache the intermediate results between search incovations such that memory allocations are reduced on multiple invocations of search.  https://github.com/seqan/product_backlog/issues/29
* sdsl index construction algorithm: https://github.com/seqan/product_backlog/issues/69
* Journal String Index: https://github.com/seqan/product_backlog/issues/112 & https://github.com/seqan/product_backlog/issues/113
* Improved search distributor: https://github.com/seqan/product_backlog/issues/109
* Dynamic Search Distributor: https://github.com/seqan/product_backlog/issues/110]
* Index construction https://github.com/seqan/product_backlog/issues/22
* Balanced Binning Directory: https://github.com/seqan/product_backlog/issues/32
* Investigate wther we can get rid of the index::text_layout (single vs collection): https://github.com/seqan/product_backlog/issues/105
* Use SIMD instructions for the minimizer computation such that the construction of the BBD and processing the queries is faster https://github.com/seqan/product_backlog/issues/38
* Allow incremental updates of the Balanaced Binning Directory such that sequences can be added/deleted or modified https://github.com/seqan/product_backlog/issues/39
* Use a generalised binning directory where a technical bin itself is not a bin with sequences but itself is a technical binning directory subsuming multiple smaller technical bins https://github.com/seqan/product_backlog/issues/37
* Add a sentinel-free implementation of the CSA to the SDSL such that we don't have to transform the text by increasing the ranks by one and increase the performance. https://github.com/seqan/product_backlog/issues/23
* Add support for generalised CSAs in the sdsl, such that we can index text collections without artificially adding a sentinel character to our indices. https://github.com/seqan/product_backlog/issues/24

* What's the point of the FM-index concepts? Are they important for users? If not, can they go away or at least be put into `::detail`?
    update: 10.03.2020 (smehringer)

    I gave this a try, but it turns out that removing the concept cannot easily be replaces by a check whther the index type is a certain template specialisisation. This needs to be re-evaluated once the search interface has been restructured and the dream_index is in plannning.

    PR: https://github.com/seqan/seqan3/pull/1632
* `bulk_contains`
    ```cpp
        [[nodiscard]] binning_bitvector const & bulk_contains(size_t const value) const & noexcept
        {
            assert(result_buffer.size() == bin_count());

            std::array<size_t, 5> bloom_filter_indices;
            std::memcpy(&bloom_filter_indices, &hash_seeds, sizeof(size_t) * hash_funs);

            for (size_t i = 0; i < hash_funs; ++i)
                bloom_filter_indices[i] = hash_and_fit(value, bloom_filter_indices[i]);

            for (size_t batch = 0; batch < bin_words; ++batch)
            {
            uint64_t * tmp = result_buffer.data();// tmp{-1ULL};
            *tmp = -1ULL;
            for (size_t i = 0; i < hash_funs; ++i)
            {
                assert(bloom_filter_indices[i] < data.size());
                *tmp &= data.get_int(bloom_filter_indices[i]);
                bloom_filter_indices[i] += 64;
            }
            ++tmp;
            }

            return result_buffer;
        }
    ```
* IBF: `bulk_contains`
    ```cpp
            if constexpr(data_layout_mode == data_layout::uncompressed)
            {
                std::memcpy(result_buffer.data(), data.data() + (idx[0] >> 6), technical_bins >> 3);
                for (size_t i = 1; i < hash_funs; ++i)
                {
                    std::memcpy(buf.data(), data.data() + (idx[i] >> 6), technical_bins >> 3);
                    result_buffer &= buf;
                }
            }
    ```

    https://github.com/seqan/seqan3/pull/920#discussion_r383786015
* The bi_fm_index_cursor does not need so many member to store positions/bounds. It should be enough to store a start and the size. (this card includes some investigative work)
* The rules and tables that define which error configuration is chosen depending on which value is ridiculously complicated: http://docs.seqan.de/seqan/3-master-user/classseqan3_1_1search__cfg_1_1max__error.html Why don't we allow arbitrary values for the errors and just say that always the strictest threshold is chosen?
* Design decision: `to_rev_iterator` for text collections reverses text indices. Change this?

    Can be resolved together with above discussion (drop single sequence support, just take collections)
    I think it should be the original order.
* search should use `thread_local` buffers for `internal_hits`, `hits` etc to prevent memory allocation on every search.
    It should be possible to provide these before via the cfg to make repeated invocations of the search faster.
    in other words: have a configuration that let's you pass an external data structure to be used as a result buffer to avoid internal re-allocation of memory
* Our bidirectional index stores the SA twice.
We should adapt our index traits to not store the SA for one of the indices in the bidirectional index.

    TODO:
    This is not necessary, but it must be investigated what is actually stored and how this can be avoided or refactored.

    First thought: We should get rid of the SA and hence always delegate the locate to the index with the sampled SA. As far as I can see, we still need BWT, Occ, C twice for second index - there are datastructures, e.g. bidirectional BWTs, that could eliminate this need.
* Refactor tests/benchmark for the `sdsl::int_vector`
    * Much of the API is not tested in the `int_vector_test` and only crashes in tests of data structures that make use of the `sdsl::int_vector`, e.g. `rrr_vector`.
    * Many tests implicitly require that something is implemented and works (the test cases do not test succinct semantics). For example, the constructor test requires that the iterator interface is correctly implemented.
    * Benchmarks for the `sdsl::int_vector` should be added/refactored.

    Implement a non-overlapping `sdsl::int_vector`.

    The standard `sdsl::int_vector` will distribute elements over word boundaries such that all bits in a word are used (no wasted bits). This is nice from a memory-centered view (no wastes space -> succinct).
    However, wasting bits makes accessing much faster.
    WIP
    https://github.com/eseiler/sdsl-lite/tree/feature/bitvector
* Nullify unnecessary overhead in SDSL.
    These are the obvious overheads:
    * SDSL-Sentinel. The SDSL uses the null character `\0`, this already increases the alphabet size by one. See https://github.com/xxsds/sdsl-lite/tree/implicit_sentinel for a branch with some work on doing an implicit sentinel, i.e. storing the position of the sentinel in the BWT instead of storing a physical sentinel inside the BWT.
    * SeqAn-Sentinel. We also append a sentinel for collections. GSA?
    * Wrapping of our datatype to make it compatible with the SDSL. (E.g., `seqan3::dna4_vector` -> `sdsl::int_vector<8>` of ranks + 1).
    * In general, we cannot construct SAs of collections in the SDSL.
* Let SDSL take any range on numeric elements for index construction, not only int_vector or C arrays.
(Reduce memory peak for index construction) https://github.com/seqan/seqan3/issues/1099
* SDSL containers should implement the STL interface.
    - [x] int_vector / bit_vector
    - [ ] int_vector_mapper
    - [ ] int_vector_buffer
* Documentation has weird language like "Roughly speaking".
* kmer index




