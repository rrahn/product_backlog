
# SeqAn goals:

* SeqAn 3.1 release -> plus release cycle
* JuSTMapper for mapping reads against populations
* iGenVar for variant calling
* Dream Index framework plus Dream-applications: DREAM-Yara/mapmap DREAM-Lambda ...
* nf-core: variant calling workflow -> mapping, calling
* generic offloading library capabilities
* large-scale read search using web-application and server based solution => Upscale the command line applications.

Great library to solve issues in bioinformatics. Write quickly nice applications with high performance.
The problem is that we need abstraction and always end up with some penalties.

## Goal 1: stable SeqAn 3 release

### Specific:
I want SeqAn to be released in a stable 3.1 version with an extensive alignment, search and I/O module. The interfaces must be stable such that users can start writing their apps with the new SeqAn versions and we can move on to extend the list of features improve performance etc.

* Release SeqAn 3.1 -> stable 3.x version of SeqAn
    * contains fast/efficient alignment
    * contains easy to use and efficient search
    * contains extendible, fast and easy-to-use I/O

### Measure: (How do I know that I finished the goal.)

A Seqan 3.1 is released containing the stable interfaces for search/alignment and I/O.

### Achievable: (How to accomplish the goal? Is it realistic?)

#### Stories

As a [role] I want [feature] so that [reason]
[action] the [result] [by|for|of|to] a(n) [object]
<!--
##### Stable dependencies !!!
Publish stable releases for our external dependencies: cereal, sdsl3 and SeqAn2, so that starting with SeqAn 3.1 release we can ship with these dependencies and also package SeqAn3 as a stand alone library.

related:

* SeqAn 2.5 is released and can coexist with SeqAn3. Specifically, I can upgrade my SeqAn2 program and use algorithms from SeqAn3 to replace steps in my current program. The other way around will be not supported. This means we can't assume that SeqAn3 data structures will work in SeqAn2 algorithms.
* The sdsl 3.0 is released and shipped by the supported package vendors. For this we need a stable EPR dictionary. What else? There are still some open problems that might affect the construction of indices and other. Do they have an effect on the interfaces?
* Cereal is released and stable and can be used within our data structures reliably and foreseeable in the future as long as we run in SeqAn3 mode. -> Optimally, we can exchange the serialisation mechanism in the backend without affecting the public API of our algorithms.
* c++20 will be released at the end of 2020 and is then fix. So we need to make sure to be fully compliant with any small changes that might happen in the meantime.

##### Compiler support !!!

Add support for gcc-11 and clang-11 to check if our current understanding and usage of modern features such as concepts is indeed working and stable. If something breaks and the solution might have an effect on the public API, then we cannot make a reliable stable release as resolving it would introduce API breaks later.

related

* all tests, snippets, benchmarks and applications based on SeqAn 3 build through and run successfully when compiled with g++-11 or clang-11.
* the output is as expected and is not effected by different rules established by the compilers. -->

<!-- ##### General documentation support !!

Make the documentation and tutorials consistent and re-evaluate the estimated timings to work through the tutorials. Users switching to 3.1 release should be able to rapidly update their programs to the new SeqAn3 version step-by-step. This means they can start with one part of their program and update it to the new functionality. Providing some recipes with a step-by-step transitioning guide would help them a lot to take advantage of the new changes.

related

* There are no dead links in the documentation:  https://github.com/seqan/seqan3/issues/232
* All the changes that were reported should be included in the tutorials: (Lydia's list)
* There are recipes that demonstrate the transition from SeqAn I/O to SeqAn3 I/O
* There are recipes that demonstrate the transition from SeqAn pairwise alignment to SeqAn3 pairwise alignment
* There are recipes that demonstrate the transition from SeqAn search to SeqAn3 search -->

<!-- ##### Additional package resource

I use conan package manager, which is the sate-of-the art package manager specifically for C++ resources. I resolve all my dependencies through conan and would like to also resolve my application dependencies to SeqAn3 in a one-package-management solution.

related:

* SeqAn3 can be installed via conan package manager.
* A recipe exists that describes how to add a SeqAn application to conan package manager. -->

<!-- ##### Logging mechanism for alignments !!!

Enable logging of the alignment configuration and errors when calling the pairwise alignment algorithm such that user can access specific runtime and configuration informations within the alignment result. For example in some cases an invalid configuration can only be checked when the alignment is invoked. At the moment it throws an exception but the user has no possibility to handle the exception, e.g. save the respective sequence and continue with the other alignments which might not be invalid.

related:
* The alignment does not throw internally but saves an error state inside of the alignment result.
* The alignment algorithm logs the configuration result inside of the alignment result and can be accessed later by the user (should this only be available in debug mode?)
* If a alignment result has an error state set, it will throw this exception when trying to access a stored member of the alignment result.
* The alignment result can be asked whether an error state was set. -->

### Acceptance Criteria

<!-- For Triage!
This section is filled later when the story is in triage:
    * Ask all relevant questions to understand the functionality, its dependencies and implications.
    * Estimate the work and split if necessary.
    * Define specifications in form of testable acceptance criteria.

In triage phase it is important to ask all relevant questions about the story such that everyone understands the functionality of the story, as well as its implications and dependencies. The story will then be estimated and possibly split into smaller components. Further, the triage team has to write down the story specifications in form of acceptance criteria that need to be tested. After completing the triage phase the issue will be handed to the dev team.
 -->

### Tasks

<!-- For Development!
This section is filled by the development team to specify the concrete tasks to solve the story.
Also update the Definition of Done accordingly.
-->

<details><summary>Definition of Done</summary>
<p>

- [ ] Implementation and design approved
- [ ] Unit tests pass
- [ ] Test coverage = 100%
- [ ] Microbenchmarks added and/or affected microbenchmarks < 5% performance drop
- [ ] API documentation added
- [ ] Tutorial/teaching material added
- [ ] Test suite compiles in less than 30 seconds (on travis)
- [ ] Changelog entry added

</p>
</details>

<!-- ##### Augment align_pairwise with meta information about the sequences !

In my application I have a complex algorithm, which selects the sequence pairs that should be aligned. To identify the correct sequences after the alignment in a concurrent environment, I would like to provide augmented information to the alignment interface such that I can identify the sequences after the alignment has been computed.

related:
* What are the lifetime guarantees of align_pairwise and should we document them?

##### General alignment interface !!!

Straighten the alignment interface such that it is stable throughout the 3.x cycle. This includes some changes to the scoring scheme concept as well as some structural changes of the scoring scheme enums.

related:

* The scoring scheme concept does not require a `score_type` member
* The amino acid scoring scheme enums are all lower case
* The score function required by the concept is provided as a CPO
* The interfaces of the aligned sequence are provided as CPOs
* aligned_sequence_concept should also require a clear_gap function -->

<!-- ##### Consider renaming of the phred alphabet !!!

Discuss and re-evaluate the naming schemes of the quality alphabets to make them either consistent with the other alphabets or consistent with the technology they represent.

The current naming scheme used to select the proper phred score is difficult to understand, because the meaning of the numbers differentiates from the meaning of the other alphabets, which is the size of the alphabet. According to this [article](https://en.wikipedia.org/wiki/FASTQ_format#Quality) following alphabets could be derived:
 - phred94: (Sanger format and used by current Illumina sequencers) rank: 0..93 ASCII 33..126
 - phred68: (Illumina 1.0 until Illumina 1.3) rank: -5..62 ASCII 59..126
 - phred63: (Illumina 1.3 until Illumina 1.8) rank: 0..62 ASCII 64..126
 - phred63+: (Illumina 1.5 until Illumina 1.8) rank: 0..2 have different meaning

Then we added a phred42, which is not represented by any of the above quality ranges but merely serves as the smallest size that can be stored together with a dna5 in an alphabet tuple using only one byte.
This makes sense, as the Illumina sequencers usually do not provide qualities higher than 40, but assemblies etc. could. -->

<!-- ##### General alphabet cleanup

* nucleotide_base converting constructor is under-constrained. Should require constexpr_writable_alphabet and have non-constexpr fallback.
* `convert_through_char_representation` is underconstrained, should require constexpr_writable_alphabet.
* `convert_through_char_representation` has SeqAn2-style wrong order of parameters (first out than in).
* make union_composition require the same char_type on alternatives
* alphabet test: remove `#include <.../all.hpp>` calls and replace them with the minimal needed includes, like in https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/mask/mask_test.cpp#L11 and https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/nucleotide/nucleotide_conversion_integration_test.cpp#L12 -->

<!-- ##### CWL support

Implement a CWL description parser such that applications using our argument parser can export a CWL file that describes the program's CLI. This is the building block to allow the automatic deployment of an application as a workflow node for the major workflow engines. It needs a docker container and a CWL tool description. Considering KNIME, there is a converter from CWL to CTD which should make it possible to provide KNIME applications. Having this, we can start integrating our tools into the nf-core. -->

<!-- ##### General Argument Parser cleanup
    * Smaller wording errors:
        https://github.com/seqan/seqan3/pull/1666#discussion_r398211518
        https://github.com/seqan/seqan3/pull/1666#discussion_r398216242
        https://github.com/seqan/seqan3/pull/1666#discussion_r398216356
    * Rename option_spec enum names to lower case
    * the value parameter to `add_option` is marked as an out-parameter, but it should be in/out as the current value is used to generate the default, right?
    * When one long_id is the prefix of another long_id, and the first one is added first, calling the second long_id will not work.
    * [Argument Parser] Follow up: Document POSIX conventions that are implemented in the argument parser: https://github.com/seqan/product_backlog/issues/201 -->

<!-- ##### File extension handling in file validator !!!

For my application I want an easy way to specify the list of extensions that are checked by the in/out file validator such that I do not end up having different extensions set in the validator and extensions set in the file.

This change greatly depends on the way how we going to pursue the I/O refactoring and should be kept in mind.
The following issues are related to this:
    * change way of providing custom file extensions to the argument parser.
    * Should the validator types be semiregular?
    * ArgParser: extension containing . accepted, but fail: https://github.com/seqan/seqan3/issues/1176
    * Sequence file input validator does not accept gzipped files: https://github.com/seqan/seqan3/issues/1591 -->

<!-- ##### Split out utility functionality from core

Split out the utility functionality from the core such that everything that is part of core is not part of the stable API and is subject to change at all time. The core provides deep functionality that is used by multiple modules. It does not have any dependencies to other modules. The utility module is a more a library inside of a library and should also be self-contained, i.e. it does not have any dependencies to any other module (also not core?).

What is core? What is utility?

related:
* Core / Utility Split: https://github.com/seqan/product_backlog/issues/160
* Make call of get in seqan3::configuration consistent https://github.com/seqan/product_backlog/issues/195 -> using get CPO
* Module  dependencies, Hannes Proposal:
    We should establish these rules:
    - modules may not depend on each other, with the following exceptions:
    - all except std and contrib may depend on core and detail
    - alignment, search and i/o may depend on alphabet and range but not on each other
* Handling of "bit-patterns"  is inconsistent. The `dynamic_bitset` takes string literals`"0101"` while the shape gets an extra strong type that is initialised with a numeric literal in binary notation: `bin_literal{0b10101}`.
    I suggest the following:
    1. rename `bin_literal` to `bits`
    2. give `bits` a constructor from number and one from string_view.
    3. introduce `0b101_bits` and `"101"_bits` as user defined literals of type `bits`.
    4. Use `bits` in the constructors of dynamic_bitset and shape.
* Put configuration stuff into a subdirectory within core/algorithm/configuration
* `seqan3::is_in_alphabet<>` (char operations) can be removed, because we have `seqan3::char_is_valid_for<>` (alphabet module) => No!!! Because it does not support the same functionality like combining them with || operator. But it could be implemented by means of the other.
* type_list_specialisation is a weird name. before the great concept rename it was called `TypeList`, so snake casing it would have been a name conflict. Are there other concept names that had this conflict?
* add_enum_bitwise_operators issues?
    * Remove the `add_enum_bitwise_operators` from the public API documentation using `//cond DEV`
    * Document the bitwise functionality in each enum that uses  `add_enum_bitwise_operators`
    * Pull `add_enum_bitwise_operators` into the detail namespace such that it works for enums that are declared    in the detail namespace
    * Do the same for strong types!
* the range returned by algorithms should be a (move-only) view. -->

<!-- ##### Cleanup the debug stream include

* Get `seqan3::debug_stream` straight once and for all: https://github.com/seqan/product_backlog/issues/63
* move debug_stream_alphabet into `alphabet/concept.hpp`, because core should not depend on alphabet.
* debug_stream and fmtflags2 still falsely appear as part of I/O module (they are part of core now) -->


<!-- ##### I/O Refactorisation !!!!!!!
* Redesign the I/O.
* Design change;
    I) The (stream, format) constructor of the files should actually be a (stream, variant<..>)-constructor so that choice of the format is still a run-time decision.
    This will make the interface much more consistent, there are virtually no drawbacks.
    II) Stream type should not be taken as template parameter either. Just take basic_istream.
    -> None of the constructors is now a template, we might be able to remove  all of the deduction guides.
* We want the files to be default constructible
* Should we use phred63 as a default quality alphabet since it accepts more values? Or maybe we could add a phred_legal_alphabet that is ohred63 and then the normal quality alphabet is still phred42 -> this depends on how we decide who is going to provide the functionality to convert the data. In the new design we might not need to handle it at all.
* Currently the bam io uses take_exactly_or_throw during the parsing extensively. However, using this functionality will cause different errors than the expected format_error, namely the unexpected_end_of_input. This is at some places inconsistent. For example if the quality string (<l_seq) is too small it might throw unexecpted_end_of_input, whereas if the quality string is too long (>l_seq) it might throw format_error. Depends on new design!
* `fields` should be variable template, not a class template.
    a) because `field` is a value and not a type
    b) because it makes `{}` necessary whenever its used
* The documentation for I/O has become very unwieldy by making the snippets copy'n'pastable. Have a look at the first snippet of `sequence_file_input`! The snippet is only supposed to illustrate that you don't need to pass template arguments (one line!!). Instead there is 20+ lines of boilerplate that is very difficult to parse (the first snippet of sequence_file_input should not use sequence_file_output!). Let's please use single-line snippets for these very simple things again. Users need to see that they can pass `"FOOBAR.fasta"` to the constructor and not `asdfasdf sdf sdfsdf; sdfsdfsdf; sequence_file_in{tmp_file/"BAR"}; asdfsdfsdfsdf` -->

<!-- ##### General I/O related issues !!

* BAM unknown reference name https://github.com/seqan/seqan3/issues/1201
* I/O Performance: https://github.com/seqan/product_backlog/issues/130
* Improve FastQ I/O https://github.com/seqan/product_backlog/issues/129
* [Sequence IO] BAM support  https://github.com/seqan/seqan3/issues/1418
* [IO] SAM/BAM performance: https://github.com/seqan/product_backlog/issues/227
* [IO] Possible improvements for the SAM/BAM format parsing: https://github.com/seqan/product_backlog/issues/226
* move `views::istreambuf` to the IO module
* `views::istreambuf` should be moved to `io/stream` from the ranges module.
* remove ostream_iterator and ostreambuf_iterator, use from std module instead.
* `output_stream_over` is a wrong name, because we are not checking if the second argument is the value type of the stream. It can be an arbitrary type as long as it can be streamable...
I am not sure we need these concepts at all. One should just not take constrained template parameters of stream, just take istream or ostream or whatever directly. It's all virtual in any case.
* The field types in `sequence_file_output` never undergo any type checking, e.g. the seq field should at least be required to be an input range over an alphabet type. -> sequence concept
* What happens on PPC with bam tests when the byte order should be converted to big endian. -->

<!-- ##### Finalise sequence and container concepts !!!

Revisit the concepts for containers that are based on the container requirements described in https://en.cppreference.com/w/cpp/named_req. The problem is that the container concepts are in many cases not really helpful to be used in generic contexts. This topic is more exhaustively described within the thesis of @h-2.
One intermediate solution was presented here:
    * Move container, sequence_container, ... into test https://github.com/seqan/product_backlog/issues/221
It suggests to put them first into test for internal use.

related:

* rename `reservible_container` to `reservable_container` (typo everywhere) -->




<!-- ##### Making range testing easier with test templates !!!

As a developer and tester I want a generic test template for ranges such that I can easily test all common features of ranges and views without repeating the same tests over and over again. Specifically, I want to be able to specify the range requirements that must be fulfilled and the test template would automatically configure the tests. It should include testing if the range can be combined with other ranges from the standard and/or the ranges library. The test template should indicate with a clear error which part was not working.

related:
    * iterator_test_template: add test for contiguous_iterator
    * Expand Iterator test template to test contiguous iterators. https://github.com/seqan/product_backlog/issues/94
    * iterator_test_template: add sized_sentinel property https://github.com/seqan/product_backlog/issues/192
    * our iterator_tag metafunction should not be necessary.
    * Reactivate test in view_interleave_test once ericniebler/range-v3#1320 ius merged. still valid?
    * Remove associated types from view classes and files classes https://github.com/seqan/seqan3/issues/914 still valid?
    * Once the range library has no implicit conversion of views anymore, search the library for `GCC diagnostic ignored "-Wdeprecated-declarations"` and remove.
    * Our pure input iterators (e.g. lazy result range in search) should become move-only once the iterator concepts do not require copy ability anymore.
    * all view concept tables should be annotated with whether they require/preserve std::semiregular. Currently all preserve this, but we need to test/annotate which also require it -> likely involves some fixing.
    * improve documentation of source views / middle views / sink views
    * move out ranges that are specific to other modules
    * remove `.cbegin()` from our views because the standard library views don't have it and it's confusing. -> Open issue: doc/howto/write_a_view/solution_view.cpp
    * our containers like `small_vector` assume the value_type is a builtin-type in many places, e.g. value-semantics, no proper moves, elements are not destructed on `pop_back` or negative resize... -->

<!-- #####  C++20 like views!!!

Make all views as similar as possible to the C++20 standard, since this gives us the highest chance that they will work together in composable view definitions and are stable for the future. It is also a good practice, since the standard documents how they are implemented and every developer can follow these guidelines.

related:

* https://github.com/seqan/product_backlog/issues/49
* some changes to `views::pairwise_combine`:
    - rename to `views::all_pairs`
    - make view factory instead of adaptor
    - have single param interface (like now)
    - add two-param interface where all pairs between the sets are generated
    - change the tuple-type to be `(seq1, seq2, id1, id2)` where id1 and id2 are numbers that indicate the original IDs of the sequences
* All views shall have explicit constructors https://github.com/seqan/seqan3/issues/941 still valid?
* change behaviour of take/drop according to P1739/P1664
* view::persist should downgrage ContiguousIterator/Range to RandomAccessIterator/Range because contiguous suggests you can directly access the memory whose lifetime might end, if not access via view::persist. Should we still offer view::persist or remove it entirely?
* Give up our own definitions in <concepts> once  ericniebler/range-v3#1322 is resolved. Still valid? -->

<!-- ##### Dream index search

Extend the search interface in order to make a DREAM-index over a large set of sequence collections searchable with the regular search interface.
Thinking about this problem might lead to some interface changes that will come sooner than later and should be considered before the 3.1 release.

related:

    * Need a DREAM-Index for starters.
    * What's the point of the FM-index concepts? Are they important for users? If not, can they go away or at least be put into `::detail`?
        update: 10.03.2020 (smehringer): I gave this a try, but it turns out that removing the concept cannot easily be replaces by a check whther the index type is a certain template specialisisation. This needs to be re-evaluated once the search interface has been restructured and the dream_index is in plannning.  PR: https://github.com/seqan/seqan3/pull/1632
    * The bi_fm_index_cursor does not need so many member to store positions/bounds. It should be enough to store a start and the size. (this card includes some investigative work)
    * The rules and tables that define which error configuration is chosen depending on which value is ridiculously complicated: http://docs.seqan.de/seqan/3-master-user/classseqan3_1_1search__cfg_1_1max__error.html Why don't we allow arbitrary values for the errors and just say that always the strictest threshold is chosen?
    * Design decision: `to_rev_iterator` for text collections reverses text indices. Change this?
    Can be resolved together with above discussion (drop single sequence support, just take collections)
        I think it should be the original orde
    * Documentation has weird language like "Roughly speaking".
    * Investigate wther we can get rid of the index::text_layout (single vs collection)  https://github.com/seqan/product_backlog/issues/105
    * Use a generalised binning directory where a technical bin itself is not a bin with sequences but itself is a technical binning directory subsuming multiple smaller technical bins https://github.com/seqan/product_backlog/issues/37 -->

<!-- ##### SDSL Index improvements

Currently, the SeqAn way of handling sequences does not fit well with SDSL index constructions.
The SeqAn3 alphabets are represented by their ranks and start with rank 0. The SDSL assumes that texts are based on chars and does not allow any text with the value `\0`, which is used internally for the sentinel.
These changes might affect the SDSL stable API and are thus relevant for the 3.1 release.

related:
 * Add support for generalised CSAs in the sdsl, such that we can index text collections without artificially adding a sentinel character to our indices. https://github.com/seqan/product_backlog/issues/24
 * Add a sentinel-free implementation of the CSA to the SDSL such that we don't have to transform the text by increasing the ranks by one and increase the performance. https://github.com/seqan/product_backlog/issues/23
 * Provide mechanism to allow specification of the SACA algorithm from outside in order to have a better control over the index construction for different use cases. https://github.com/seqan/product_backlog/issues/25 -->


#### Open tasks:
    * Module dependencies?
    * External dependencies:
        * SeqAn2 release:
            - To make it combinable with SeqAn3 we need a way to allow seqan range types to be used by seqan3 algorithms. The easiest way for this would be to provide a seqan::SimpleType wrapper to comply with the seqan3::alphabet and seqan3::writeable_alphabet. Must it be constexpr?
            Only allow one direction: from SeqAn2 -> SeqAn3
        * cereal stable: We need a stable Cereal release, or we need an alternative.
            - evaluate until 24 Dec 2020 -> If not reevaluate the situation with cereal
            - We need to enable versioned cerealisation https://uscilab.github.io/cereal/serialization_functions.html#explicit-versioning
            - cereal concepts should all go into detail as they are not relevant for users. The cereal archives should also get aliases in `seqan3::`.
            - Reevaluate cereal: https://github.com/USCiLab/cereal/issues/563
            Likely we have to make a full fork or switch to a different project. Possibilities:
            Alternatives:
                - yas
                - cista++
                - See also https://github.com/thekvs/cpp-serializers
        * c++20 has to be released before we can fix our implementation on the current standard.
        * We need a stable SDSL release: (we are not maintaining the entire SDLS but only the relevant parts)
            - [ ] EPR dictionary!

    * Infrastructure:
        * Added Compiler support: gcc-11 and clang
        * modularize tests *per modelling concept one test template*
            - for example `fm_index`, `bi_fm_index` https://github.com/seqan/seqan3/blob/
        * Add Support for Conan Package management
        * detect dead links in documentation: https://github.com/seqan/seqan3/issues/232
        * Googletest should track master

    * Documentation:
        * Improve the tutorials based on the stuff Lydia did

    * Alignment
        * <span style="color:red">Concepts for sequence and alignment: https://github.com/seqan/product_backlog/issues/60</span>
        * What are the lifetime guarantees of align_pairwise and should we document them?
        * <span style="color:red">A complete new way to call alignment_pairwise.</span>
        * Scoring scheme
            - <span style="color:red">requirement for `::score_type` should be remove from `scoring_scheme` concept. The score type is whatever is returned by the score function!</span>
            - <span style="color:red">Rename scoring scheme enums (e.g BLOSSUM) to lower case</span>
        * Add basic vectorisation support for the dna global affine alignment: https://github.com/seqan/product_backlog/issues/54
        * Vectorised alignment with protein sequences: https://github.com/seqan/product_backlog/issues/206
        * Simd profile score https://github.com/seqan/product_backlog/issues/210
        * Improved Edit Distance: https://github.com/seqan/product_backlog/issues/117
    * alphabet
        * phred alphabet naming:
            phred63 seems arbritrary because the phred scale is actually of size 94. So it should either be phred94 or maybe just phred?
            phred42 was chosen to be able to use a dna5-phred42 alphabet_tuple that still fits in one byte. This could be named small_phred.
        * nucleotide_base converting constructor is under-constrained. Should require constexpr_writable_alphabet and have non-constexpr fallback.
        * `convert_through_char_representation` is underconstrained, should require constexpr_writable_alphabet.
        * `convert_through_char_representation` has SeqAn2-style wrong order of parameters (first out than in).
        * make union_composition require the same char_type on alternatives
        * alphabet test: remove `#include <.../all.hpp>` calls and replace them with the minimal needed includes, like in https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/mask/mask_test.cpp#L11 and https://github.com/seqan/seqan3/blob/53cce95d0743d453b73e6ece549a86e00dce7552/test/unit/alphabet/nucleotide/nucleotide_conversion_integration_test.cpp#L12

    * argument parser
        * Smaller wording errors:
            https://github.com/seqan/seqan3/pull/1666#discussion_r398211518
            https://github.com/seqan/seqan3/pull/1666#discussion_r398216242
            https://github.com/seqan/seqan3/pull/1666#discussion_r398216356
        * Rename option_spec enum names to lower case
        * the value parameter to `add_option` is marked as an out-parameter, but it should be in/out as the current value is used to generate the default, right?
        * CWL support -> discuss with mr-c
        * change way of providing custom file extensions to the argument parser.
        * When one long_id is the prefix of another long_id, and the first one is added first, calling the second long_id will not work.
        * Should the validator types be semiregular?
        * [Argument Parser] Follow up: Document POSIX conventions that are implemented in the argument parser: https://github.com/seqan/product_backlog/issues/201
        * ArgParser: extension containing . accepted, but fail: https://github.com/seqan/seqan3/issues/1176
        * Sequence file input validator does not accept gzipped files: https://github.com/seqan/seqan3/issues/1591
    * Core
        * Core / Utility Split: https://github.com/seqan/product_backlog/issues/160
        * Get `seqan3::debug_stream` straight once and for all: https://github.com/seqan/product_backlog/issues/63
        * Make call of get in seqan3::configuration consistent https://github.com/seqan/product_backlog/issues/195 -> using get CPO
        * type_list_specialisation is a weird name. before the great concept rename it was called `TypeList`, so snake casing it would have been a name conflict. Are there other concept names that had this conflict?
        * add_enum_bitwise_operators issues?
            - [ ] Remove the `add_enum_bitwise_operators` from the public API documentation using `//cond DEV`
            - [ ] Document the bitwise functionality in each enum that uses  `add_enum_bitwise_operators`
            - [ ] Pull `add_enum_bitwise_operators` into the detail namespace such that it works for enums that are declared    in the detail namespace
            - [ ] Do the same for strong types!
        * the range returned by algorithms should be a (move-only) view. `seqan3::char_is_valid_for<>` (alphabet module) Should it not be just aliased? No!!! Because it does not support the same functionality. But it could be implemented by means of the other.
        * move debug_stream_alphabet into `alphabet/concept.hpp`, because core should not depend on alphabet.
        * Module  dependencies, Hannes Proposal:
            We should establish these rules:
            - modules may not depend on each other, with the following exceptions:
            - all except std and contrib may depend on core and detail
            - alignment, search and i/o may depend on alphabet and range but not on each other
        * Put configuration stuff into a subdirectory within core/algorithm/configuration
        * Handling of "bit-patterns"  is inconsistent. The `dynamic_bitset` takes string literals`"0101"` while the shape gets an extra strong type that is initialised with a numeric literal in binary notation: `bin_literal{0b10101}`.
            I suggest the following:
            1. rename `bin_literal` to `bits`
            2. give `bits` a constructor from number and one from string_view.
            3. introduce `0b101_bits` and `"101"_bits` as user defined literals of type `bits`.
            4. Use `bits` in the constructors of dynamic_bitset and shape.
    * I/O
        * Redesign the I/O.
        * Design change;
            I) The (stream, format) constructor of the files should actually be a (stream, variant<..>)-constructor so that choice of the format is still a run-time decision.
            This will make the interface much more consistent, there are virtually no drawbacks.
            II) Stream type should not be taken as template parameter either. Just take basic_istream.
            -> None of the constructors is now a template, we might be able to remove  all of the deduction guides.
        * We want the files to be default constructible
        * BAM unknown reference name https://github.com/seqan/seqan3/issues/1201
            * I/O Performance: https://github.com/seqan/product_backlog/issues/130
            * Improve FastQ I/O https://github.com/seqan/product_backlog/issues/129
            * [Sequence IO] BAM support  https://github.com/seqan/seqan3/issues/1418
            * [IO] SAM/BAM performance: https://github.com/seqan/product_backlog/issues/227
            * [IO] Possible improvements for the SAM/BAM format parsing: https://github.com/seqan/product_backlog/issues/226
        * Should we use phred63 as a default quality alphabet since it accepts more values? Or maybe we could add a phred_legal_alphabet that is ohred63 and then the normal quality alphabet is still phred42
        * Currently the bam io uses take_exactly_or_throw during the parsing extensively. However, using this functionality will cause different errors than the expected format_error, namely the unexpected_end_of_input. This is at some places inconsistent. For example if the quality string (<l_seq) is too small it might throw unexecpted_end_of_input, whereas if the quality string is too long (>l_seq) it might throw format_error.
        * `fields` should be variable template, not a class template.
        a) because `field` is a value and not a type
        b) because it makes `{}` necessary whenever its used
        * The field types in `sequence_file_output` never undergo any type checking, e.g. the seq field should at least be required to be an input range over an alphabet type. -> sequence concept
        * The documentation for I/O has become very unwieldy by making the snippets copy'n'pastable. Have a look at the first snippet of `sequence_file_input`! The snippet is only supposed to illustrate that you don't need to pass template arguments (one line!!). Instead there is 20+ lines of boilerplate that is very difficult to parse (the first snippet of sequence_file_input should not use sequence_file_output!). Let's please use single-line snippets for these very simple things again. Users need to see that they can pass `"FOOBAR.fasta"` to the constructor and not `asdfasdf sdf sdfsdf; sdfsdfsdf; sequence_file_in{tmp_file/"BAR"}; asdfsdfsdfsdf`
        * `output_stream_over` is a wrong name, because we are not checking if the second argument is the value type of the stream. It can be an arbitrary type as long as it can be streamable...
        I am not sure we need these concepts at all. One should just not take constrained template parameters of stream, just take istream or ostream or whatever directly. It's all virtual in any case.
        * `views::istreambuf` should be moved to `io/stream` from the ranges module.
        * debug_stream and fmtflags2 still falsely appear as part of I/O module (they are part of core now)
        * remove ostream_iterator and ostreambuf_iterator, use from std module instead.
        * What happens on PPC with bam tests when the byte order should be converted to big endian.
    * Ranges
        * Move container, sequence_container, ... into test https://github.com/seqan/product_backlog/issues/221
        * iterator_test_template: add test for contiguous_iterator
        * Expand Iterator test template to test contiguous iterators. https://github.com/seqan/product_backlog/issues/94
        * iterator_test_template: add sized_sentinel property https://github.com/seqan/product_backlog/issues/192
        * Our pure input iterators (e.g. lazy result range in search) should become move-only once the iterator concepts do not require copy ability anymore.
        * our containers like `small_vector` assume the value_type is a builtin-type in many places, e.g. value-semantics, no proper moves, elements are not destructed on `pop_back` or negative resize...
        * some changes to `views::pairwise_combine`:
            - rename to `views::all_pairs`
            - make view factory instead of adaptor
            - have single param interface (like now)
            - add two-param interface where all pairs between the sets are generated
            - change the tuple-type to be `(seq1, seq2, id1, id2)` where id1 and id2 are numbers that indicate the original IDs of the sequences
        * remove `.cbegin()` from our views because the standard library views don't have it and it's confusing.
        * move `views::istreambuf` to the IO module
        * rename `reservible_container` to `reservable_container` (typo everywhere)
        * all view concept tables should be annotated with whether they require/preserve std::semiregular. Currently all preserve this, but we need to test/annotate which also require it -> likely involves some fixing.
        * our iterator_tag metafunction should not be necessary. can be removed?
        * Once the range library has no implicit conversion of views anymore, search the library for `GCC diagnostic ignored "-Wdeprecated-declarations"` and remove.
        * Give up our own definitions in <concepts> once  ericniebler/range-v3#1322 is resolved. Still valid?
        * Reactivate test in view_interleave_test once ericniebler/range-v3#1320 ius merged. still valid?
        * Remove associated types from view classes and files classes https://github.com/seqan/seqan3/issues/914 still valid?
        * All views shall have explicit constructors https://github.com/seqan/seqan3/issues/941 still valid?
        * change behaviour of take/drop according to P1739/P1664
        * improve documentation of source views / middle views / sink views
        * move out ranges that are specific to other modules
        * Hiwi work: Take out all the huge iterators from the parent class. still valid?
        * view::persist should downgrage ContiguousIterator/Range to RandomAccessIterator/Range because contiguous suggests you can directly access the memory whose lifetime might end, if not access via view::persist. Should we still offer view::persist or remove it entirely?
        * aligned_sequence_concept should also require a clear_gap function
    * Search
        * What's the point of the FM-index concepts? Are they important for users? If not, can they go away or at least be put into `::detail`?
            update: 10.03.2020 (smehringer): I gave this a try, but it turns out that removing the concept cannot easily be replaces by a check whther the index type is a certain template specialisisation. This needs to be re-evaluated once the search interface has been restructured and the dream_index is in plannning.  PR: https://github.com/seqan/seqan3/pull/1632
        * The bi_fm_index_cursor does not need so many member to store positions/bounds. It should be enough to store a start and the size. (this card includes some investigative work)
        * The rules and tables that define which error configuration is chosen depending on which value is ridiculously complicated: http://docs.seqan.de/seqan/3-master-user/classseqan3_1_1search__cfg_1_1max__error.html Why don't we allow arbitrary values for the errors and just say that always the strictest threshold is chosen?
        * Design decision: `to_rev_iterator` for text collections reverses text indices. Change this?
        Can be resolved together with above discussion (drop single sequence support, just take collections)
            I think it should be the original orde
        * Documentation has weird language like "Roughly speaking".

### Relevant:

* Does this seem worthwhile? -> yes
* Is this the right time? -> maybe
* Does this match our other efforts/needs? -> yes
* Am I the right person to reach this goal? -> yes
* Is it applicable in the current socio-economic environment? -> maybe

### Time-Bound:

* 2 dev cycles:
    * In 6 months.
    * release candidate: 26.03.2021
    * release-date: 09.04.2021

## Goal 2: JuST mapper

### Specific

I want a proof-of-concept application that can use the referentially compressed sequences and abstracts the stream algorithms in such a way that they work on this interface, i.e. any context-based streaming algorithm can be applied to this framework. I want to be able to modify the sequence index without waiting too long for rebuilding the index structures but make it immediately available for the search.
This means I can add/remove or change any index inside of the represented set.

### Measure:

JuSTMap 1.0 is released.
A paper about this project is submitted to a journal or conference.

### Achievable: (How to accomplish the goal? Is it realistic?)

need a journal sequence with following capabilities:
 * insert a character/string
 * erase a character/string
 * replace a character/string
 * construct it from an alignment: special scoring scheme.
 * Iterable -> models std::ranges::range

need a journal sequence index data structure:
 * add sequence incrementally -> the base sequence must be the same.
 * remove sequence
 * exchange sequence
 * built over existing sequence collection
 * IBF distributor to find index segment
 * return a journal index window
 * apply algorithm over journal index window

application that:
 * maps single-end reads
 * maps paired-end reads
 * searches them in the application
 * allows to make changes to an existing index
 * rebuilt index from scratch given a set
 * built index from vcf file

### Relevant

* Does this seem worthwhile? -> yes
* Is this the right time? -> yes
* Does this match our other efforts/needs? -> yes
* Am I the right person to reach this goal? -> yes
* Is it applicable in the current socio-economic environment? -> yes

### Time-Bound:

* 1 dev cycles:
    * In 3 months.
    * release candidate: 01.01.2021
    * release-date: 18.01.2021

## Goal 3: iGenVar

Someone has to work into nf-core and look through different workflows.
Then we need someone that investigates the performance of these workflows and analyse them by some specific metrics.
Then we need to analyse

### Specific

We want a variant caller that collects the ideas of various different callers to provide a full-stack caller which can deal with short- and long-reads and improves the recall of existing variants.
This caller works on large-scale data very well and can be installed via different installation processes.

### Measure:

iGenVar 1.0 is released.
A paper about this project is submitted to a journal or conference.

### Achievable: (How to accomplish the goal? Is it realistic?)

* needs to read in bam files and also bam index
* needs vcf/bcf output
* stream over the bam file and read records
* what is needed to decide for certain information? What needs to be parsed?
* calls deletions on short reads and long-reads
* calls insertions on short reads and long-reads
* identifies different variations based on the result of deletion/insertion calling

### Relevant

* Does this seem worthwhile? -> yes
* Is this the right time? -> yes
* Does this match our other efforts/needs? -> yes
* Am I the right person to reach this goal? -> yes
* Is it applicable in the current socio-economic environment? -> yes

### Time-Bound:

* 2 dev cycles:
    * In 6 months.
    * release candidate: 26.03.2021
    * release-date: 09.04.2021

## Goal 4: Dream Index framework

### Specific

We want a variant caller that collects the ideas of various different callers to provide a full-stack caller which can deal with short- and long-reads and improves the recall of existing variants.
This caller works on large-scale data very well and can be installed via different installation processes.

### Measure:

iGenVar 1.0 is released.
A paper about this project is submitted to a journal or conference.

### Achievable: (How to accomplish the goal? Is it realistic?)

* needs to read in bam files and also bam index
* needs vcf/bcf output
* stream over the bam file and read records
* what is needed to decide for certain information? What needs to be parsed?
* calls deletions on short reads and long-reads
* calls insertions on short reads and long-reads
* identifies different variations based on the result of deletion/insertion calling

### Relevant

* Does this seem worthwhile? -> yes
* Is this the right time? -> yes
* Does this match our other efforts/needs? -> yes
* Am I the right person to reach this goal? -> yes
* Is it applicable in the current socio-economic environment? -> yes

### Time-Bound:

* 2 dev cycles:
    * In 6 months.
    * release candidate: 26.03.2021
    * release-date: 09.04.2021
