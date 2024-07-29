---
layout: page
title: Systems Engineering Project 2
description: Development of a Food Printing System
img: assets/img/evebot1.png
importance: 1
category: work
related_publications: false
---

<!-- Describe the objective of SEP2 -->
## Objective
The objective of this project is to develop a system that enhances the presentation, customization, and efficiency of food preparation, while considering the needs of various stakeholders, such as restaurants, caterers, and consumers.

## Outcome
EASY, a food printer that is capable of printing 2D graphical images on a tray of pastries autonomously.

<!-- To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    --- -->

<!-- Video of EASY in operation -->
<div class="videorow">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/easyvidnosound.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>
<!-- Caption of EASY video -->
<div class="caption">
    The EASY Printer printing the Chanel logo on a tray of Pain Au Chocolat Pastries.
</div>


<!-- Show photo of the output of the Chanel logo and other prints -->
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/chanelchocprint.jpg" title="Chanel Logo on Pain Au Chocolat" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nicherchocprint.jpg" title="Nicher Logo on Pain Au Chocolat" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nicherplainprint.jpg" title="Nicher Logo on Croisaant" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Printed logos and images on various pastries.
</div>





<!-- Skills Deployed -->
## Skills Deployed & Responsibilities
- Project Management
    - Agile
- Systems Engineering
- Mechanical Design
- Embedded Systems


<!-- Photo of EASY printer -->
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/easyprinter1.jpg" title="Nicher and EASY Development Team" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The EASY Printer.
</div>

<!-- Functionalities of EASY printer -->
## Functionalities of EASY printer
1. Prints on a tray of pastries without any pre-alignment by the user, allowing pastries to be placed in any order.
2. Able to adapt to the arrangement of pastries on the tray autonomously and printing accordingly.
3. Capable of printing on various types of pastries, such as croissants, pain au chocolat etc.

Through our research into the workflow of bakeries, we identified several key steps in their daily operations.
- Once the pastries are baked, the chef removes the trays from the oven and places them on a cooling rack. 
- After the pastries have sufficiently cooled, the chef decorates each pastry, a labor-intensive and precise task.
- Finally, the decorated and branded pastries are transferred to the display area, where they are showcased for sale to customers.

Understanding this workflow allowed us to identify critical points where the EASY Printer could add value. By integrating our system into the decoration and branding phase, we aimed to automate and streamline this process, making it more efficient and less labor-intensive.

<!-- Stakeholder engagement -->
## Stakeholder engagement

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nichergroupphoto.jpg" title="Nicher group photo" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left to right: Muslinmin, Kaushik, Reuben, Yugendren, Melvin (Owner of Nicher), and Stafford.
</div>

### Our Stakeholder
[Nicher](https://nicher.com.sg/) is a local bakery and café located at 60 Springside Walk, #01-08, Singapore 786020. Run by an Ex-MBS pâtissier, Melvin, Nicher specializes in artisanal pastries such as Almond Croissants and Kouign Amann.

Through our stakeholder engagement with Nicher, we discovered:
- Nicher is located in a residential area with low footfall, meaning residents not a significant customer base for Nicher.
- Faces increasing costs in raw materials (e.g., butter).
- Nicher aims for organic brand growth rather than aggressive social media advertising. Previous attempts to generate trends, such as with mini croissants, were short-lived and did not result in sustained customer growth or retention.
- Nicher uses a minimalist packaging design for its pastries where the only branded element is the 'Nicher' label on the washi tape on the boxes.

Additionally, Nicher also serves its pastries at coporate events as part of its revenue stream and to boost brand recognition. As well as, expressed the need to stand out and differentiate its pastries from other bakeries. At corporate events, there are other food vendors as well, nicher wants to stand out among the other vendors.

In addition to retail sales, Nicher boosts its brand recognition and revenue by serving pastries at corporate events. However, at these events, there are also other food vendors. Nicher expressed a strong need to differentiate its pastries and stand out among competitors to attract more customers and enhance brand visibility as most types of pastries look similar.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/nicherdiscussion.jpg" title="Nicher and EASY Development Team" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

During the development of the EASY Printer prototype, the team regularly engaged with Nicher to gather insights, feedback and to ensure the solution met their needs. These engagements provided valuable perspectives that guided our development process which included successful trials where Nicher's brand logo was printed on their pastries. The stakeholder found the results acceptable and provided valuable feedback.

Insights:
- Printing logos on pastries adds value by enhancing brand recognition. Nicher's unique branding on their pastries helps them stand out in a competitive market.
- Automating the printing process is particularly beneficial for single-owned businesses like Nicher, as it reduces the time and effort required for manual branding.
- Larger chain bakeries, such as Delifrance, could also benefit from the EASY Printer prototype due to their higher production volumes.
    - We identified additional potential stakeholders who could benefit from the EASY Printer. This opens up new avenues for collaboration and application across various segments of the food industry.


<!-- SEP2 Technical Documentation Section -->
## [For more information, view the Technical Documentation of SEP2 here](https://reubenlow.github.io/blog/2024/sep2docs/)


{% raw %}


{% endraw %}
