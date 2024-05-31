<h1>M&M Detection Project</h1>
<p>This project utilizes OpenCV to detect M&M candies in images and specifically identifies blue M&Ms.</p>
<h2>Overview</h2>
<p>The goal of this project is to detect M&M candies in images, highlight them, and count the number of blue M&Ms. The project includes two main functions:</p>
<ul>
    <li><code>pair_plot</code>: To display two images side by side.</li>
    <li><code>find_total</code>: To detect circles representing M&Ms, identify blue M&Ms, and highlight them.</li>
</ul>
<h2>Functions</h2>
<h3><code>pair_plot</code></h3>
<p>Displays two images side by side for comparison.</p>
<p><strong>Parameters:</strong></p>
<ul>
    <li><code>img1</code>: The first image to display.</li>
    <li><code>img2</code>: The second image to display.</li>
    <li><code>title1</code>: The title for the first image (default is "Original").</li>
    <li><code>title2</code>: The title for the second image (default is "Filtered").</li>
</ul>
<p><strong>Functionality:</strong></p>
<ul>
    <li>Creates a figure with a size of 16x16 inches.</li>
    <li>Adds two subplots (1 row, 2 columns).</li>
    <li>Converts the color space of the images from BGR to RGB (since OpenCV uses BGR by default and Matplotlib uses RGB).</li>
    <li>Displays the images and sets the titles.</li>
    <li>Hides the axis for a cleaner view.</li>
</ul>
<h3><code>find_total</code></h3>
<p>Detects M&Ms in an image, highlights blue M&Ms, and displays the results.</p>
<p><strong>Parameters:</strong></p>
<ul>
    <li><code>path</code>: The file path to the image.</li>
</ul>
<p><strong>Functionality:</strong></p>
<ol>
    <li>
        <p><strong>Initialization and Image Loading:</strong></p>
        <pre><code>blue_mnms = 0
Oraginal_image = cv2.imread(path)
image = cv2.imread(path)</code></pre>
        <p>Initialize a counter <code>blue_mnms</code> to zero, which will count the number of blue M&Ms. Load the original image from the given <code>path</code> and store it in <code>Oraginal_image</code> and <code>image</code>.</p>
    </li>
    <li>
        <p><strong>Error Handling for Image Loading:</strong></p>
        <pre><code>if image is None:
    print(f"Error: Unable to load image from the path: {path}")
    return</code></pre>
        <p>Check if the image was loaded successfully. If not, print an error message and exit the function.</p>
    </li>
    <li>
        <p><strong>Grayscale Conversion:</strong></p>
        <pre><code>gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)</code></pre>
        <p>Convert the image from BGR color space to grayscale. This is necessary for the Hough Circle Transform to detect circles effectively.</p>
    </li>
    <li>
        <p><strong>Gaussian Blur:</strong></p>
        <pre><code>blurred_image = cv2.GaussianBlur(gray_image, (15, 15), 2)</code></pre>
        <p>Apply Gaussian Blur to the grayscale image to reduce noise and improve the accuracy of circle detection.</p>
    </li>
    <li>
        <p><strong>Circle Detection using Hough Transform:</strong></p>
        <pre><code>circles = cv2.HoughCircles(
    blurred_image,
    cv2.HOUGH_GRADIENT,
    dp=2,
    minDist=60,
    param1=26,
    param2=31,
    minRadius=56,
    maxRadius=60
)</code></pre>
        <p>Use the Hough Circle Transform to detect circles in the blurred image.</p>
        <ul>
            <li><code>dp=2</code>: The inverse ratio of the accumulator resolution to the image resolution.</li>
            <li><code>minDist=60</code>: Minimum distance between detected centers.</li>
            <li><code>param1=26</code>: Higher threshold for the Canny edge detector.</li>
            <li><code>param2=31</code>: Threshold for center detection.</li>
            <li><code>minRadius=56</code>: Minimum circle radius.</li>
            <li><code>maxRadius=60</code>: Maximum circle radius.</li>
        </ul>
    </li>
    <li>
        <p><strong>Circle Processing and Color Checking:</strong></p>
        <pre><code>if circles is not None:
    circles = np.uint16(np.around(circles))
    for i in circles[0, :]:
        center_coordinates = (int(i[0]), int(i[1]))
