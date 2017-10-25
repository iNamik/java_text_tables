# java\_text\_tables

**Text Table Library in Java**

## About

The `Java Text Tables` library is the next generation of my original [Java Text Table Formatter](https://github.com/iNamik/Java-Text-Table-Formatter) project.

This new project hopes to offer more extensibility and composability, while still providing the ease-of-use of the original project.

## Examples

### SimpleTable Example

 `SimpleTable` is Optimized for quick table construction without having to know the final table dimensions beforehand.  It Allows you to build a table one row/cell at a time.

    // NOTE: Apply vertical alignment FIRST !
    //
    SimpleTable s = SimpleTable.of()
        .nextRow()
            .nextCell()
                .addLine("Left")
                .addLine("Top")
                .applyToCell(TOP_ALIGN .withHeight(height))
                .applyToCell(LEFT_ALIGN.withWidth (width ).withChar('^'))
            .nextCell()
                .addLine("Center")
                .addLine("Top")
                .applyToCell(TOP_ALIGN        .withHeight(height))
                .applyToCell(HORIZONTAL_CENTER.withWidth (width ))
            .nextCell()
                .addLine("Right")
                .addLine("Top")
                .applyToCell(TOP_ALIGN  .withHeight(height))
                .applyToCell(RIGHT_ALIGN.withWidth (width ))
        .nextRow()
            .nextCell()
                .addLine("Left")
                .addLine("Center")
                .applyToCell(VERTICAL_CENTER.withHeight(height))
                .applyToCell(LEFT_ALIGN     .withWidth (width ))
            .nextCell()
                .addLine("Center")
                .addLine("Center")
                .applyToCell(VERTICAL_CENTER  .withHeight(height))
                .applyToCell(HORIZONTAL_CENTER.withWidth (width ).withChar('.'))
            .nextCell()
                .addLine("Right")
                .addLine("Center")
                .applyToCell(VERTICAL_CENTER.withHeight(height))
                .applyToCell(RIGHT_ALIGN    .withWidth (width ))
        .nextRow()
            .nextCell()
                .addLine("Left")
                .addLine("Bottom")
                .applyToCell(BOTTOM_ALIGN.withHeight(height))
                .applyToCell(LEFT_ALIGN  .withWidth (width ))
            .nextCell()
                .addLine("Center")
                .addLine("Bottom")
                .applyToCell(BOTTOM_ALIGN     .withHeight(height))
                .applyToCell(HORIZONTAL_CENTER.withWidth (width ))
            .nextCell()
                .addLine("Right")
                .addLine("Bottom")
                .applyToCell(BOTTOM_ALIGN.withHeight(height))
                .applyToCell(RIGHT_ALIGN .withWidth (width ).withChar('_'))
        ;

    //
    // NOTE: SimpleTable makes creating the table easy, but you will need to convert it
    // into a GridTable in order to perform further operations
    // (like adding a border or printing)
    //
    
    // Convert to grid
    //
    GridTable g = s.toGrid();

    // Add simple border
    //
    g = Border.of(Border.Chars.of('+', '-', '|')).apply(g);

    
    // Print the table to System.out
    //
    Util.print(g);

The above code should result in the following output:

    +----------+----------+----------+
    |Left^^^^^^|  Center  |     Right|
    |Top^^^^^^^|   Top    |       Top|
    |^^^^^^^^^^|          |          |
    |^^^^^^^^^^|          |          |
    |^^^^^^^^^^|          |          |
    |^^^^^^^^^^|          |          |
    +----------+----------+----------+
    |          |..........|          |
    |          |..........|          |
    |Left      |..Center..|     Right|
    |Center    |..Center..|    Center|
    |          |..........|          |
    |          |..........|          |
    +----------+----------+----------+
    |          |          |__________|
    |          |          |__________|
    |          |          |__________|
    |          |          |__________|
    |Left      |  Center  |_____Right|
    |Bottom    |  Bottom  |____Bottom|
    +----------+----------+----------+

### GridTable Example

 `GridTable` Offers a more powerful table builder (compared to SimpleTable), but requires knowledge of the final table dimensions at construction, along with specifying coordinates `(row, col)` when manipulating cells.

    GridTable g = GridTable.of(3, 3)
        .put(0, 0, Cell.of("Left"  , "Top"   ))
        .put(0, 1, Cell.of("Center", "Top"   ))
        .put(0, 2, Cell.of("Right" , "Top"   ))

        .put(1, 0, Cell.of("Left"  , "Center"))
        .put(1, 1, Cell.of("Center", "Center"))
        .put(1, 2, Cell.of("Right" , "Center"))

        .put(2, 0, Cell.of("Left"  , "Bottom"))
        .put(2, 1, Cell.of("Center", "Bottom"))
        .put(2, 2, Cell.of("Right" , "Bottom"))

        .applyToRow(0, TOP_ALIGN      .withHeight(height))
        .applyToRow(1, VERTICAL_CENTER.withHeight(height))
        .applyToRow(2, BOTTOM_ALIGN   .withHeight(height))

        .apply(0, 0, LEFT_ALIGN.withWidth(width).withChar('^'))
        .apply(1, 0, LEFT_ALIGN)
        .apply(2, 0, LEFT_ALIGN)

        .apply(0, 1, HORIZONTAL_CENTER.withWidth(width))
        .apply(1, 1, HORIZONTAL_CENTER.withChar('.'))
        .apply(2, 1, HORIZONTAL_CENTER)

        .apply(0, 2, RIGHT_ALIGN.withWidth(width))
        .apply(1, 2, RIGHT_ALIGN)
        .apply(2, 2, RIGHT_ALIGN.withChar('_'))
        ;
    
    // Add fancy border
    //
    g = Border.DOUBLE_LINE.apply(g);
    
    // Print the table to System.out
    //
    Util.print(g);

The above code should result in the following output:

    ╔══════════╦══════════╦══════════╗
    ║Left^^^^^^║  Center  ║     Right║
    ║Top^^^^^^^║   Top    ║       Top║
    ║^^^^^^^^^^║          ║          ║
    ║^^^^^^^^^^║          ║          ║
    ║^^^^^^^^^^║          ║          ║
    ║^^^^^^^^^^║          ║          ║
    ╠══════════╬══════════╬══════════╣
    ║          ║..........║          ║
    ║          ║..........║          ║
    ║Left      ║..Center..║     Right║
    ║Center    ║..Center..║    Center║
    ║          ║..........║          ║
    ║          ║..........║          ║
    ╠══════════╬══════════╬══════════╣
    ║          ║          ║__________║
    ║          ║          ║__________║
    ║          ║          ║__________║
    ║          ║          ║__________║
    ║Left      ║  Center  ║_____Right║
    ║Bottom    ║  Bottom  ║____Bottom║
    ╚══════════╩══════════╩══════════╝
*NOTE* Spaces between Border Character are due to HTML styling and would not appear in a terminal

## Importing Into Your Project

### Snapshot

    <repositories>
      <repository>
        <id>oss-sonatype</id>
        <name>oss-sonatype</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.github.inamik.text.tables</groupId>
        <artifactId>inamik-text-tables</artifactId>
        <version>1.0-SNAPSHOT</version>
      </dependency>
    </dependencies>

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :D

## Credits

* David Farrell (DavidPFarrell@yahoo.com)

## License

Licensed under The MIT License (MIT), see LICENSE.txt
