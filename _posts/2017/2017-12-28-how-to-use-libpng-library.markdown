---
title: How to use libpng library
layout: post
author:     "JongHyun"
header-img: "img/machine_learning_backboard.jpg"
categories: [C, Image Process]
---
### What is libpng?
<p> 
    <span style="display:inline-block; width: 30px;"></span>libpng is a kind of official png library. You can download at this <a href="http://www.libpng.org/pub/png/libpng.html">site</a>. Because it is platform independent library, we can use this library very easily. I used this library in my project to convert from bmp to png, from png to bmp. Although there are another png library like lodepng, libpng is the fastest library in my experience. There was experiement comparing the speed of each png library. You can find it at this <a href="https://caedesnotes.wordpress.com/2015/03/23/comparing-performance-stb_image-vs-libjpeg-turbo-libpng-and-lodepng/">link</a>.
</p>


### Read PNG file
<p> 
    <span style="display:inline-block; width: 30px;"></span>The easy way to start libpng is reading png file by using libpng. The output array will be value each pixel which can be used to make bmp foramt. Let's start reading.
    <pre><code class="language-c line-numbers"  numbering>FILE *fp = fopen(file_name, "rb");
if(!fp){
    // dealing with error
}
// 1. Read first 8 bytes to check the file is png or not
unsigned char header[9];
int number_to_check = 8;
fread(header, 1, number_to_check, fp);
int is_png = !png_sig_cmp(header, 0, number);
if(!is_png){
    // dealing with error
}

// 2. Create png struct pointer
png_structp png_ptr = png_create_read_struct(PNG_LIBPNG_VER_STRING, NULL, NULL, NULL);
if (!png_ptr) {
    // dealing with error
}
png_infop info_ptr = png_create_info_struct(png_ptr);
if (!info_ptr) {
    png_destroy_read_struct(&png_ptr, (png_infopp)NULL, (png_infopp)NULL);
    // dealing with error
}
png_infop end_info = png_create_info_struct(png_ptr);
if (!end_info) {
    png_destroy_read_struct(&png_ptr, &info_ptr, (png_infopp)NULL);
    // dealing with error
}
// Set file pointer to png struct 
png_init_io(png_ptr, fp); 
// Inicidate how many bytes we already read;
png_set_sig_bytes(png_ptr, number_to_check); 

// 3. Read png info
png_read_info(png_ptr, info_ptr);
png_uint_32 width, height;
int bit_depth, color_type, interlace_type, compression_type, filter_method;
png_get_IHDR(png_ptr, info_ptr, &width, &height, &bit_depth, &color_type, \
        &interlace_type, NULL, NULL);

// 4. Read png image data
// Set row pointer which will take pixel value from png file
png_bytepp row_pointers = (png_bytepp)png_malloc(png_ptr, sizeof(png_bytepp) * height);
for (int i = 0; i < height; i++) {
    row_pointers[i] = (png_bytep)png_malloc(png_ptr, width * pixel_size);
}
// Set row pointer to the png struct
png_set_rows(png_ptr, info_ptr, row_pointers);
// Read png image data and save in row pointer
// After reading the image, you can deal with the image data with row pointers
png_read_image(png_ptr, row_pointers);
png_read_end(pbox.png_ptr, pbox.end_info);
// Finish the reading process and reset each pointer
png_destroy_read_struct(&pbox.png_ptr, &pbox.info_ptr, &pbox.end_info);
</code></pre>
</p>

### Read PNG format array(convert png array to bmp array)
<p> 
    <span style="display:inline-block; width: 30px;"></span>Of course there are also another situation that we want to convert png format array to bmp format. The overall process is similiar with reading png file. The difference is that we need to use <code>png_set_read_fn</code> and declare our own reading function. Also, we need to use our own io struct instead of using file pointer.
<pre><code class="language-c line-numbers"  numbering>// Declare our own io struct
typedef struct {
    png_bytep buffer;
    png_uint_32 bufsize;
    png_uint_32 current_pos;
} MEMORY_READER_STATE;
// This function will be used for reading png data from array
static void read_data_memory(png_structp png_ptr, png_bytep data, png_uint_32 length) 
{
    MEMORY_READER_STATE *f = png_get_io_ptr(png_ptr);
    if (length > (f->bufsize - f->current_pos))
        png_error(png_ptr, "read error in read_data_memory (loadpng)");
    memcpy(data, f->buffer + f->current_pos, length);
    f->current_pos += length;
}

// In the reading function in the previous code........
// Before this part is same upto step 2 except png_init_io function
// since we will not use file pointer 

MEMORY_READER_STATE memory_reader_state;
// png_source is array which have png data
memory_reader_state.buffer = (png_bytep)png_source;
memory_reader_state.bufsize = png_data_size;
memory_reader_state.current_pos = number_to_check;
// set our own read_function
png_set_read_fn(png_ptr_p2b, &memory_reader_state, read_data_memory);
png_set_sig_bytes(png_ptr, number_to_check); 

// After this part, it is same with step 3 in the previous code
</code></pre>
</p>



### Write PNG file
<p> 
    <span style="display:inline-block; width: 30px;"></span>Next step is writing png file from pixel data or data array of bmp file. It is almost same with reading png file.
    <pre><code class="language-c line-numbers"  numbering>FILE * fp2 = fopen(output_name, "wb");
if (!fp2) {
    // dealing with error
}

// 1. Create png struct pointer
png_structp png_ptr = png_create_write_struct(PNG_LIBPNG_VER_STRING, NULL, NULL, NULL);
if (!png_ptr){
    // dealing with error
}
png_infop info_ptr = png_create_info_struct(png_ptr);
if (!info_ptr) {
    png_destroy_write_struct(&png_ptr, (png_infopp)NULL);
    // dealing with error
}

// 2. Set png info like width, height, bit depth and color type
//    in this example, I assumed grayscale image. You can change image type easily
int width = user_image_width;
int height = user_image_height;
int bit_depth = user_bit_depth
png_init_io(png_ptr, fp2);
png_set_IHDR(png_ptr, info_ptr, width, height, bit_depth, \
    PNG_COLOR_TYPE_GRAY, PNG_INTERLACE_NONE, \
    PNG_COMPRESSION_TYPE_DEFAULT, PNG_FILTER_TYPE_DEFAULT);

// 3. Convert 1d array to 2d array to be suitable for png struct
//    I assumed the original array is 1d
png_bytepp row_pointers = (png_bytepp)png_malloc(png_ptr, sizeof(png_bytepp) * height);
for (int i = 0; i < height; i++) {
    row_pointers[i] = (png_bytep)png_malloc(png_ptr, width);
}
for (int hi = 0; hi < height; hi++) {
    for (int wi = 0; wi < width; wi++) {
        // bmp_source is source data that we convert to png
        row_pointers[hi][wi] = bmp_source[wi + width * hi];
    }
}

// 4. Write png file
png_write_info(png_ptr, info_ptr);
png_write_image(png_ptr, row_pointers);
png_write_end(png_ptr, info_ptr);
png_destroy_write_struct(&png_ptr, &info_ptr);
</code></pre>
</p>

### Write PNG format array(convert bmp array to png array)
<p> 
    <span style="display:inline-block; width: 30px;"></span>You can also convert pixel data array to png format. Simliar with reading step, we need to use our own writing function and <code>png_set_write_fn</code>. We will not use file output.
<pre><code class="language-c line-numbers"  numbering>// Declare our own io struct
typedef struct {
    png_bytep buffer;
    png_uint_32 bufsize;
} MEMORY_WRITER_STATE;
// This function will be used for writing png data 
static void write_data_memory(png_structp png_ptr, png_bytep data, png_uint_32 length) {
    MEMORY_WRITER_STATE * p = png_get_io_ptr(png_ptr);
    png_uint_32 nsize = p->bufsize + length;

    if (p->buffer)
        p->buffer = (png_bytep)realloc(p->buffer, nsize);
    else
        p->buffer = (png_bytep)malloc(nsize);

    if (!p->buffer)
        png_error(png_ptr, "WriteError");
    memcpy(p->buffer + p->bufsize, data, length);
    p->bufsize += length;
}

// png_set_write_fn also require user's flush function. If we set just NULL to 
// png_set_write_fn, it will process its default flush function. So, it will be
// better to just declare like this.
void user_png_flush(png_structp png_ptr) {
}

// In the writing function in the previous code........
// Before this part is same upto step 3 except png_init_io function

MEMORY_WRITER_STATE memory_writer_state;
memory_writer_state.buffer = NULL;
memory_writer_state.bufsize = 0;
png_set_write_fn(png_ptr, &memory_writer_state, write_data_memory, user_png_flush);

// We will get png array at the buffer in memory_writer_state struct.
// After this part, it is same with step 3 in the previous code
</code></pre>
</p>
<p> 
    <span style="display:inline-block; width: 30px;"></span>Because I didn't deal with specific error function or care about memory usage much, there might be some small problem in this code. However, it works well and we can start with this basic structure to other application. You can find more in detail in this <a href="http://www.libpng.org/pub/png/libpng-1.2.5-manual.html">libpng manual</a>. And any question is welcome.
</p>