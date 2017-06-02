---
title: API Reference
kind: documentation
---

<%
  # Some shortcuts for the columns.
  left_side_div  = '<div class="col-xs-12 col-md-6 api-left">'
  right_side_div = '<div class="col-xs-12 col-md-6 api-right">'

  no_args = '<em>This end point takes no JSON arguments.</em>'
  no_response = '<em>This end point does not return JSON on successful requests.</em>'
%>

<div class="container api-page">
  <div class="row floating-header" data-spy="affix" data-offset-top="60">
    <h1><%= @item[:title] %> </h1>
    <div class="btn-toolbar">
      <div class="btn-group api-section-links btn-group-sm">
        <a class="btn btn-default gradient" href="#authentication">Auth</a>
        <a class="btn btn-default gradient" href="#errors">Errors</a>
        <a class="btn btn-default gradient" href="#ratelimiting">Rate Limiting</a>
        <a class="btn btn-default gradient" href="#metrics">Metrics</a>
        <a class="btn btn-default gradient" href="#events">Events</a>
        <a class="btn btn-default gradient" href="#service_checks">Checks</a>
        <a class="btn btn-default gradient" href="#monitors">Monitors</a>
        <a class="btn btn-default gradient" href="#downtimes">Downtimes</a>
        <a class="btn btn-default gradient" href="#timeboards">Timeboards</a>
        <a class="btn btn-default gradient" href="#screenboards">Screenboards</a>
        <a class="btn btn-default gradient" href="#hosts">Hosts</a>
        <a class="btn btn-default gradient" href="#tags">Tags</a>
        <a class="btn btn-default gradient" href="#search">Search</a>
        <a class="btn btn-default gradient" href="#comments">Comments</a>
        <a class="btn btn-default gradient" href="#users">Users</a>
        <a class="btn btn-default gradient" href="#graphs">Graphs</a>
        <a class="btn btn-default gradient" href="#embeds">Embeds</a>
      </div>
      </div>
      <div class="btn-toolbar">
      <div class="btn-group language-links btn-group-sm">
        <div lang="console" class="lang-btn btn btn-default gradient">Shell</div>
        <div lang="python" class="active lang-btn btn btn-default gradient">Python</div>
        <div lang="ruby" class="lang-btn btn btn-default gradient">Ruby</div>
      </div>
      <div class="btn-group btn-group-sm">
        <a class="btn btn-default gradient" href="#troubleshooting">Troubleshooting</a>
      </div>
    </div>
  </div>

  <div class="scrollable-api-docs">

    <!--
    =====================================================================
      OVERVIEW
    ====================================================================
    -->

    <div class="int-anchor" id="overview"></div><a href="#overview"><h3>Overview</h3></a>
    <div class="row">
      <%= left_side_div %>
        This section details Datadog's HTTP API. It makes it easy to get data in and
        out of Datadog.

        The Datadog API uses resource-oriented URLs, uses status codes to indicate the
        success or failure of requests and returns JSON from all requests. Let's dive
        in and see how it works.
      </div>
      <%= right_side_div %>
        <h5>Libraries</h5>
        There are many client libraries that wrap the Datadog API. <a href="/libraries/">Check them out.</a>

        <h5>API Endpoint</h5>
        <code>https://app.datadoghq.com/api/</code>
      </div>
    </div>

    <!--
   =====================================================================
      AUTHENTICATION
    ====================================================================
    -->
    <div class="int-anchor" id="authentication"></div><a href="#authentication"><h3>Authentication</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          All requests to Datadog's API must be authenticated. Requests that write data require
          <em>reporting access</em> and require an <code>API key</code>. Requests that read data
          require <em>full access</em> and also require an <code>application key</code>.
        </p>

        <p>You can manage your account's API and application keys <a href="https://app.datadoghq.com/account/settings#api">here</a>.</p>
      </div>
      <%= right_side_div %>
        <h5>Example</h5>
        <%= snippet_code_block "api-auth.sh" %>
        <%= snippet_code_block "api-auth.py" %>
        <%= snippet_code_block "api-auth.rb" %>
      </div>
    </div>

    <!--
   =====================================================================
      Success & Errors
    ====================================================================
    -->

    <div class="int-anchor" id="errors"></div><a href="#errors"><h3>Success and Errors</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        The Datadog API uses HTTP status codes to indicate the success or failure
        of a request.
        </p>

        <p>
        An error indicates that the service did not successfully handle your request. In addition to the status code, the response may contain a JSON object with an errors array containing more detailed error messages. Note: When using libraries, some errors may throw exceptions rather than return JSON objects.
        </p>

        <p>
  If the service is able to handle your request, but some issues are present (e.g. using a deprecated API or API version), the HTTP status code will indicate success and the response body will contain the expected result with the addition of a warnings array containing detailed warning messages.
        </p>



      </div>
      <%= right_side_div %>
        <h5>Status Codes</h5>
        <ul>
        <% STATUS_CODES.values.sort.each do |value| %>
          <li> <code><%= value %> </code></li>
        <% end %>
        </ul>
        <h5>Example Error Response</h5>
        <%= snippet_code_block "api-error.json", :static => true, :highlight => false, :nocomments => true %>
        <h5>Example Warning Response</h5>
        <%= snippet_code_block "api-warning.json", :static => true, :highlight => false, :nocomments => true %>
      </div>
    </div>



    <!--
   =====================================================================
      Rate Limiting
    ====================================================================
    -->

    <div class="int-anchor" id="ratelimiting"></div><a href="#ratelimiting"><h3>Rate Limiting</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        Some of our API endpoints are rate limited.  Once you exceed a certain number of requests in a certain time period we return an error.
        </p>
        <p>
        For rate limited API endpoints we return headers so you can know how close you are to your limit.  If you exceed your limit, you can review these headers to determine when you will be able to try again.
        </p>
        <p>
        Rate limits can be increased from defaults by <a href="/help">contacting the Datadog Support team</a>.
        </p>
      </div>
      <%= right_side_div %>
        <h5>Rate Limit Headers</h5>
        <ul>
          <li><code>X-RateLimit-Limit</code>number of requests allowed in a time period</li>
          <li><code>X-RateLimit-Period</code>length of time in seconds for resets (calendar aligned)</li>
          <li><code>X-RateLimit-Remaining</code>number of allowed requests left in current time period</li>
          <li><code>X-RateLimit-Reset</code>time in seconds until next reset</li>
        </ul>
      </div>
    </div>


    <!--
   =====================================================================
    Metrics
    ====================================================================
    -->

    <div class="int-anchor" id="metrics"></div><a href="#metrics"><h3>Metrics</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The metrics end-point allows you to:
        </p>
        <ul>
          <li>Post metrics data so it can be graphed on Datadog's dashboards</li>
          <li>Query metrics from any time period</li>
        </ul>
        <p>
          As occurs within the Datadog UI, a graph can only contain a set number of points and as the timeframe over which a metric is viewed increases, aggregation between points will occur to stay below that set number.
        </p>
        <p>
          Thus, if you are querying for larger timeframes of data, the points returned will be more aggregated. The max granularity within Datadog is one point per second, so if you had submitted points at that interval and requested a very small interval from the query API (in this case, probably less than 100 seconds), you could end up getting all of those points back. Otherwise, our algorithm tries to return about 150 points per any given time window, so you'll see coarser and coarser granularity as the amount of time requested increases. We do this time aggregation via averages.
        </p>
      </div>
    </div>

    <div class="lang-specific lang-specific-console">
    <div class="int-anchor" id="metrics-list"></div><a href="#metrics-list"><h4>Get list of active metrics</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Get the list of actively reporting metrics from a given time until now. This endpoint is not available in the Python and Ruby libraries.
        </p>

        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("from", "seconds since the unix epoch", :lang => 'console') %>
        </ul>

      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET https://app.datadoghq.com/api/v1/metrics</code>
        <h5>Example Request</h5>
          <%= snippet_code_block "api-metrics-list.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-metrics-list.sh" %>
      </div>
    </div>
    </div>

    <div class="int-anchor" id="metrics-post"></div><a href="#metrics-post"><h4>Post time series points</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The metrics end-point allows you to post time-series data that can be
          graphed on Datadog's dashboards.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument('series', 'A JSON array of metrics where each item in the array contains the following arguments:', :lang => 'console') %>
          <%= argument('series', 'To submit multiple metrics, you may pass a JSON array where each item in the array contains the following arguments. To submit a single metric, you may pass the following arguments as separate arguments.', :default => 'None', :lang => 'python') %>
          <%= argument 'metric', 'The name of the time series' %>
          <li>
            <strong>points [required]</strong>
            <div>A JSON array of points. Each point is of the form:
              <div>
                <code>[[POSIX_timestamp, numeric_value], ...]</code>
              </div>
              Note that the timestamp should be in seconds, must be current, and the numeric value is a 32bit float gauge-type value.
              Current is defined as not more than 10 minutes in the future or more than 1 hour in the past.
            </div>
          </li>
          <%= argument('host', 'The name of the host that produced the metric.', :default => 'None' ) %>
          <%= argument('tags', 'A list of tags associated with the metric.', :default => 'None' ) %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST https://app.datadoghq.com/api/v1/series</code>
        <h5>Example Request</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_code_block "api-metrics-post.#{f}" %>
        <% end %>
      </div>
    </div>

    <div class="int-anchor" id="metrics-query"></div><a href="#metrics-query"><h4>Query time series points</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to query for metrics from any time period.
        </p>

        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("from", "seconds since the unix epoch", :lang => 'console') %>
          <%= argument("to", "seconds since the unix epoch", :lang => 'console') %>
          <%= argument("start", "seconds since the unix epoch", :lang => 'python') %>
          <%= argument("end", "seconds since the unix epoch", :lang => 'python') %>
          <%= argument("query", "The query string") %>
        </ul>
        <h5>Query Language</h5>
        <p>
          Any query used for a graph can be used here. See <a href="/graphing/">here</a> for more details. The time between from and to should be less than 24 hours. If it is longer, you will receive points with less granularity.
        </p>

      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET https://app.datadoghq.com/api/v1/query</code>
        <h5>Example Request</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_code_block "api-metrics-query.#{f}" %>
        <% end %>
        <h5>Example Response</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_result_code_block "api-metrics-query.#{f}" %>
        <% end %>
      </div>
    </div>

    <div class="int-anchor" id="metrics-metadata-get"></div><a href="#metrics-metadata-get"><h4>View metric metadata</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The metrics metadata endpoint allows you to get metadata about a specific metric.
        </p>

        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/metrics/:metric_name</code>
        <h5>Example Request</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_code_block "api-metric-metadata-get.#{f}" %>
        <% end %>
        <h5>Example Response</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_result_code_block "api-metric-metadata-get.#{f}" %>
        <% end %>
      </div>
    </div>

    <div class="int-anchor" id="metrics-metadata-update"></div><a href="#metrics-metadata-update"><h4>Edit metric metadata</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The metrics metadata endpoint allows you to edit fields of a metric's metadata.
        </p>

        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("type", "metric type such as 'gauge' or 'rate'", {:default => 'None'}) %>
          <%= argument("description", "string description of the metric", {:default => 'None'}) %>
          <%= argument("short_name", "short name string of the metric", {:default => 'None'}) %>
          <%= argument("unit", "primary unit of the metric such as 'byte' or 'operation'", {:default => 'None'}) %>
          <%= argument("per_unit", "'per' unit of the metric such as 'second' in 'bytes per second'", {:default => 'None'}) %>
          <%= argument("statsd_interval", "if applicable, statds flush interval in seconds for the metric", {:default => 'None'}) %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT /api/v1/metrics/:metric_name</code>
        <h5>Example Request</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_code_block "api-metric-metadata-update.#{f}" %>
        <% end %>
        <h5>Example Response</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_result_code_block "api-metric-metadata-update.#{f}" %>
        <% end %>
      </div>
    </div>

    <!--
   =====================================================================
    Events
    ====================================================================
    -->

    <div class="int-anchor" id="events"></div><a href="#events"><h3>Events</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The events service allows you to programatically post events to the
          stream and fetch events from the stream.
        </p>
      </div>
    </div>
    <div class="int-anchor" id="events-post"></div><a href="#events-post"><h4>Post an Event</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to post events to the stream. You can tag them, set priority and event aggregate them with other events.
        </p>
        <h5>Arguments</h5>
        <% valid_sources = "nagios, hudson, jenkins, my apps, feed, chef, puppet, git, bitbucket, fabric, capistrano"  %>
        <ul class="arguments">
          <% %w(console python).each do |l| %>
            <%= argument "title", 'The event title. Limited to 100 characters.', :lang => l %>
            <%= argument "text", "The body of the event. Limited to 4000 characters. The text supports "+'<a href="/guides/markdown/">markdown</a>.', :lang => l %>
          <% end %>
          <%= argument 'msg_text', 'The text for the message. Limited to 4000 characters. The text supports '+'<a href="/guides/markdown/">markdown</a>.', :lang => 'ruby' %>
          <%= argument "msg_title", 'The event title. Limited to 100 characters.', :lang => 'ruby', :default => "''" %>
          <%= argument("date_happened", "POSIX timestamp of the event.", {:default => 'now'}) %>
          <%= argument("priority", "The priority of the event ('normal' or 'low').", {:default => "'normal'"}) %>
          <%= argument("host", "Host name to associate with the event. Any tags associated with the host will also be applied to this event.", {:default => 'None'}) %>
          <%= argument("tags", "A list of tags to apply to the event.", {:default => 'None'}) %>
          <%= argument("alert_type", '"error", "warning", "info" or "success".', {:default => "'info'"}) %>
          <%= argument("aggregation_key", 'An arbitrary string to use for aggregation, max length of 100 characters. If you specify a key, all events using that key will be grouped together in the Event Stream.', {:default => 'None'}) %>
          <%= argument("source_type_name", 'The type of event being posted. <div>Options: ' + valid_sources + '</div>', {:default => 'None'}) %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/events</code>
        <h5>Example Request</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_code_block "api-events-post.#{f}" %>
        <% end %>
        <h5>Example Response</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_result_code_block "api-events-post.#{f}" %>
        <% end %>
      </div>
    </div>
    <div class="int-anchor" id="events-get"></div><a href="#events-get"><h4>Get an Event</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to query for event details.
        </p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/events/:event_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-events-get.py" %>
        <%= snippet_code_block "api-events-get.sh" %>
        <%= snippet_code_block "api-events-get.rb" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-events-get.py" %>
        <%= snippet_result_code_block "api-events-get.sh" %>
        <%= snippet_result_code_block "api-events-get.rb" %>
      </div>
    </div>

    <div class="int-anchor" id="events-delete"></div><a href="#events-delete"><h4>Delete an Event</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to delete an event.
        </p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/events/:event_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-events-delete.py" %>
        <%= snippet_code_block "api-events-delete.sh" %>
        <%= snippet_code_block "api-events-delete.rb" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-events-delete.py" %>
        <%= snippet_result_code_block "api-events-delete.sh" %>
        <%= snippet_result_code_block "api-events-delete.rb" %>
      </div>
    </div>

    <div class="int-anchor" id="events-get-all"></div><a href="#events-get-all"><h4>Query the Event Stream</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The event stream can be queried and filtered by time, priority, sources and tags.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument "start", "POSIX timestamp" %>
          <%= argument "end", "POSIX timestamp" %>
          <%= argument "priority", "'low' or 'normal'", :default => "None" %>
          <%= argument "sources", "A comma separated string of sources", :default => "None" %>
          <%= argument "tags", "A comma separated string of tags", :default => "None" %>
        </ul>

      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/events</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-events-stream.py" %>
        <%= snippet_code_block "api-events-stream.rb" %>
        <%= snippet_code_block "api-events-stream.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-events-stream.py" %>
        <%= snippet_result_code_block "api-events-stream.rb" %>
        <%= snippet_result_code_block "api-events-stream.sh" %>
      </div>
    </div>


    <!--
   =====================================================================
    Service Checks
    ====================================================================
    -->

    <div class="int-anchor" id="service_checks"></div><a href="#service_checks"><h3>Service Checks</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The service check endpoint allows you to post check statuses for use with
          monitors.
        </p>
      </div>
    </div>
    <div class="int-anchor" id="checks-post"></div><a href="#checks-post"><h4>Post a Check Run</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <% valid_statuses = "'0': OK, '1': WARNING, '2': CRITICAL, '3': UNKNOWN" %>
          <%= argument 'check', 'The text for the message' %>
          <%= argument 'host_name', 'The name of the host submitting the check' %>
          <%= argument 'status', 'An integer for the status of the check. <div>Options: ' + valid_statuses + '</div>' %>
          <%= argument 'timestamp', 'POSIX timestamp of the event.', {:default => 'now'} %>
          <%= argument 'message', 'A description of why this status occurred', {:default => 'None'} %>
          <%= argument 'tags', 'A list of key:val tags for this check', {:default => 'None'} %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/check_run</code>
        <h5>Example Request</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_code_block "api-checks-post.#{f}" %>
        <% end %>
        <h5>Example Response</h5>
        <% %w{py sh rb}.each do |f| %>
          <%= snippet_result_code_block "api-checks-post.#{f}" %>
        <% end %>
      </div>
    </div>

    <!--
    =====================================================================
    Monitors
    ====================================================================
    -->

    <div class="int-anchor" id="monitors"></div><a href="#monitors"><h3>Monitors</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>Monitors allow you to watch a metric or check that you care about,
        notifying your team when some defined threshold is exceeded. Please
        refer to the <a href="/guides/monitors">Guide to Monitors</a> for more
        information on creating monitors.</p>
      </div>
    </div>

    <!-- Create monitor -->
    <div class="int-anchor" id="monitor-create"></div><a href="#monitor-create"><h4>Create a monitor</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <li>
            <strong>type [required]</strong>
            <div>The type of the monitor, chosen from:
              <ul>
                <li><code>metric alert</code></li>
                <li><code>service check</code></li>
                <li><code>event alert</code></li>
                <li><code>composite</code></li>
              </ul>
            </div>
          </li>
          <li>
            <strong>query [required]</strong>
            <div>The query defines when the monitor will trigger. Query syntax depends
            on what type of monitor you are creating:</div>

            <h5>Metric Alert Query</h5>

            <code>time_aggr(time_window):space_aggr:metric{tags} [by {key}] operator #</code>
            <ul class="arguments">
              <li><code>time_aggr</code> avg, sum, max, min, change, or pct_change</li>
              <li><code>time_window</code> last_#m (5, 10, 15, or 30),
              last_#h (1, 2, or 4), or last_1d</li>
              <li><code>space_aggr</code> avg, sum, min, or max</li>
              <li><code>tags</code> one or more tags (comma-separated), or *</li>
              <li><code>key</code> a 'key' in key:value tag syntax; defines a
              separate alert for each tag in the group (multi-alert)</li>
              <li><code>operator</code> &lt;, &lt;=, &gt;, &gt;=, ==, or !=</li>
              <li><code>#</code> an integer or decimal number used to set the
              threshold</li>
            </ul>
            <div>If you are using the <em>change</em> or <em>pct_change</em>
             time aggregator, you can instead use
             <code>change_aggr(time_aggr(time_window), timeshift):space_aggr:metric{tags} [by {key}] operator #</code>
             with:
             <ul class="arguments">
              <li><code>change_aggr</code> change, pct_change</li>
              <li><code>time_aggr</code> avg, sum, max, min</li>
              <li><code>time_window</code> last_#m (1, 5, 10, 15, or 30),
              last_#h (1, 2, or 4), or last_#d (1 or 2)</li>
              <li><code>timeshift</code> #m_ago (5, 10, 15, or 30),
              #h_ago (1, 2, or 4), or 1d_ago</li>
             </ul>
            </div>
            <div>You can also use this to create an outlier monitor using the following query: <code>avg(last_30m):outliers(avg:system.cpu.user{role:es-events-data} by {host}, 'dbscan', 7) > 0</code></div>

            <h5>Service Check Query</h5>
            <code>"check".over(tags).last(count).count_by_status()</code>
            <ul class="arguments">
              <li><code>check</code> name of the check, e.g. datadog.agent.up</li>
              <li><code>tags</code> one or more quoted tags (comma-separated),
              or "*". e.g.: <code>.over("env:prod", "role:db")</code></li>
              <li><code>count</code> must be at >= your max threshold (defined
              in the <code>options</code>). e.g. if you want to notify on 1
              critical, 3 ok and 2 warn statuses count should be 3.</li>
            </ul>

            <h5>Event Alert Query</h5>
            <code>events('sources:nagios status:error,warning priority:normal tags: "string query"').rollup("count").last("1h")"</code>
            <ul class="arguments">
              <li><code>event</code>, the event query string:
                <ul class="arguments">
                  <li><code>string_query</code> free text query to match against event title and text.</li>
                  <li><code>sources</code> event sources (comma-separated).</li>
                  <li><code>status</code> event statuses (comma-separated). Valid options: error, warn, and info.</li>
                  <li><code>priority</code> event priorities (comma-separated). Valid options: low, normal, all.</li>
                  <li><code>host</code> event reporting host (comma-separated).</li>
                  <li><code>tags</code> event tags (comma-separated).</li>
                  <li><code>excluded_tags</code> exluded event tags (comma-separated).</li>
                </ul>
              </li>
              <li><code>rollup</code> the stats rollup method. <code>count</code> is the only supported method now.</li>
              <li><code>last</code> the timeframe to roll up the counts. Examples: 60s, 4h. Supported timeframes: s, m, h and d.</li>
            </ul>

            <h5>Composite Query</h5>
            <div><code>12345 && 67890</code>, where <code>12345</code> and <code>67890</code> are the IDs of non-composite monitors</div>

          </li>
          <%= argument 'name', "The name of the alert.",
              :default => "dynamic, based on query" %>
          <%= argument 'message', "A message to include with notifications for
              this monitor. Email notifications can be sent to specific users by
              using the same '@username' notation as events.",
              :default => "dynamic, based on query" %>
          <%= argument 'tags', "A list of tags to associate with your monitor.
              When getting all monitor details via the API, you can use the <code>monitor_tags</code> argument to filter results by these tags.
              Tags using the \"service\" key (e.g. <code>service:my-app</code>) will be visible in the corresponding \"service\" field when editing the monitor in the Datadog UI. Tags without the \"service\" key will only be available via the API and will not be visible or editable in the Datadog UI.",
              :default => "empty list" %>

          <li>
            <strong>options [optional]</strong>
            <div>A dictionary of options for the monitor. There are options that
            are common to all types as well as options that are specific to
            certain monitor types.</div>

            <h5>Common Options</h5>

            <ul class="arguments">
              <li><code>silenced</code> dictionary of scopes to timestamps or
              <code>None</code>. Each scope will be muted until the given POSIX
              timestamp or forever if the value is <code>None</code>.
              <p>Default: <code>None</code></p>
              <p>Examples:

                <ul>
                  <li>To mute the alert completely:
                      <p><code>{'*': None}</code></p></li>
                  <li>To mute <code>role:db</code> for a short time:
                      <p><code>{'role:db': 1412798116}</code></p></li>
                </ul>
              </p>
              </li>

              <li><code>notify_no_data</code> a boolean indicating whether this
              monitor will notify when data stops reporting.
              <p>Default: <code>false</code></p></li>

              <li><code>new_host_delay</code> Time (in seconds) to allow a host
              to boot and applications to fully start before starting the
              evaluation of monitor results. Should be a non negative integer.
              <p>Default: <code>300</code></p></li>

              <li><code>no_data_timeframe</code> the number of minutes before a
              monitor will notify when data stops reporting. Must be at least
              1x the monitor timeframe for metric alerts or 2 minutes for
              service checks.
              <p>Default: <code>1x timeframe for metric alerts, 2 minutes for
              service checks</code></p></li>

              <li><code>timeout_h</code> the number of hours of the monitor not
              reporting data before it will automatically resolve from a
              triggered state.
              <p>Default: <code>None</code></p></li>

              <li><code>require_full_window</code> a boolean indicating whether
              this monitor needs a full window of data before it's evaluated. We
              highly recommend you set this to <code>False</code> for sparse
              metrics, otherwise some evaluations will be skipped.
              <p>Default: <code>True</code> for "on average", "at all times" and
              "in total" aggregation. <code>False</code> otherwise.</p></li>

              <li><code>renotify_interval</code> the number of minutes after
              the last notification before a monitor will re-notify on the
              current status. It will only re-notify if it's not resolved.
              <p>Default: <code>None</code></p></li>

              <li><code>escalation_message</code> a message to include with a
              re-notification. Supports the '@username' notification we allow
              elsewhere. Not applicable if <code>renotify_interval</code> is
              <code>None</code>.
              <p>Default: <code>None</code></p></li>

              <li><code>notify_audit</code> a boolean indicating whether tagged
              users will be notified on changes to this monitor.
              <p>Default: <code>False</code></p></li>

              <li><code>locked</code> a boolean indicating whether changes to
              to this monitor should be restricted to the creator or admins.
              <p>Default: <code>False</code></p></li>

              <li><code>include_tags</code> a boolean indicating whether
              notifications from this monitor will automatically insert its
              triggering tags into the title.
              <p>Default: <code>True</code></p>
              <p>Examples:

                <ul>
                  <li>True:
                      <p><code>[Triggered on {host:h1}] Monitor Title</code></p></li>
                  <li>False:
                      <p><code>[Triggered] Monitor Title</code></p></li>
                </ul>
              </p>
              </li>


            </ul>

            <h5>Metric Alert Options</h5>

            <em>These options only apply to metric alerts.</em>

            <ul class="arguments">
              <li><code>thresholds</code> a dictionary of thresholds by threshold
              type. Currently we have two threshold types for metric alerts:
              critical and warning. Critical is defined in the query, but can
              also be specified in this option. Warning threshold can only be
              specified using the thresholds option.
              <p>Example: <code>{'critical': 90, 'warning': 80}</code></p>
              </li>
              <li><code>evaluation_delay</code> Time (in seconds) to delay
              evaluation, as a non-negative integer. For example, if the value
              is set to 300 (5min), the timeframe is set to last_5m and the
              time is 7:00, the monitor will evaluate data from 6:50 to 6:55.
              This is useful for AWS CloudWatch and other backfilled metrics to
              ensure the monitor will always have data during evaluation.
              </li>
            </ul>

            <h5>Service Check Options</h5>

            <em>These options only apply to service checks and will be ignored
            for other monitor types.</em>

            <ul class="arguments">
              <li><code>thresholds</code> a dictionary of thresholds by status.
              Because service checks can have multiple thresholds, we don't
              define them directly in the query.
              <p>Default: <code>{'ok': 1, 'critical': 1, 'warning': 1}</code></p>
              </li>
            </ul>

          </li>
        </ul>
        </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/monitor</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-create.py" %>
        <%= snippet_code_block "api-monitor-create.rb" %>
        <%= snippet_code_block "api-monitor-create.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-create.py" %>
        <%= snippet_result_code_block "api-monitor-create.rb" %>
        <%= snippet_result_code_block "api-monitor-create.sh" %>
        </div>
      </div>


    <!-- Show monitor -->
    <div class="int-anchor" id="monitor-get-details"></div><a href="#monitor-get-details"><h4>Get a monitor's details</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'group_states', "If this argument is
          set, the returned data will include additional information (if
          available) regarding the specified group states, including the
          last notification timestamp, last resolution timestamp and
          details about the last time the monitor was triggered. The
          argument should include a string list indicating what, if any,
          group states to include. Choose one or more from 'all', 'alert',
          'warn', or 'no data'. Example: 'alert,warn'",
          :default => "None" %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/monitor/:monitor_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-show.py" %>
        <%= snippet_code_block "api-monitor-show.rb" %>
        <%= snippet_code_block "api-monitor-show.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-show.py" %>
        <%= snippet_result_code_block "api-monitor-show.rb" %>
        <%= snippet_result_code_block "api-monitor-show.sh" %>
      </div>
    </div>

    <!-- Edit monitor -->
    <div class="int-anchor" id="monitor-edit"></div><a href="#monitor-edit"><h4>Edit a monitor</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'query', "The metric query to alert on." %>
          <%= argument 'name', "The name of the monitor.",
              :default => "dynamic, based on query" %>
          <%= argument 'message', "A message to include with notifications for
              this monitor. Email notifications can be sent to specific users by
              using the same '@username' notation as events.",
              :default => "dynamic, based on query" %>
          <%= argument 'options', "Refer to the create monitor documentation for
              details on the available options.", :default => "None" %>
          <%= argument 'tags', "A list of tags to associate with your monitor.
              This can help you categorize and filter monitors.",
              :default => "empty list" %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT /api/v1/monitor/:monitor_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-edit.py" %>
        <%= snippet_code_block "api-monitor-edit.rb" %>
        <%= snippet_code_block "api-monitor-edit.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-edit.py" %>
        <%= snippet_result_code_block "api-monitor-edit.rb" %>
        <%= snippet_result_code_block "api-monitor-edit.sh" %>
      </div>
    </div>

    <!-- Delete monitor -->
    <div class="int-anchor" id="monitor-delete"></div><a href="#monitor-delete"><h4>Delete a monitor</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/monitor/:monitor_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-delete.py" %>
        <%= snippet_code_block "api-monitor-delete.rb" %>
        <%= snippet_code_block "api-monitor-delete.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-delete.py" %>
        <%= snippet_result_code_block "api-monitor-delete.rb" %>
        <%= snippet_result_code_block "api-monitor-delete.sh" %>
      </div>
    </div>

    <!-- Show all monitors -->
    <div class="int-anchor" id="monitor-get-all"></div><a href="#monitor-get-all"><h4>Get all monitor details</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'group_states', "If this argument is
          set, the returned data will include additional information (if
          available) regarding the specified group states, including the
          last notification timestamp, last resolution timestamp and
          details about the last time the monitor was triggered. The
          argument should include a string list indicating what, if any,
          group states to include. Choose one or more from 'all', 'alert',
          'warn', or 'no data'. Example: 'alert,warn'",
          :default => "None" %>
          <%= argument 'name', "A string to filter monitors by name",
          :default => "None" %>
          <%= argument 'tags', "A comma separated list indicating
          what tags, if any, should be used to filter the list of monitors
          by scope, e.g. <code>host:host0</code>. For more information, see the <code>tags</code> parameter for the appropriate <code>query</code> argument in the <a href='#monitor-create'>Create a monitor</a> section above.",
          :default => "None" %>
          <%= argument 'monitor_tags', "A comma separated list indicating
          what service and/or custom tags, if any, should be used to filter the list of monitors. Tags created in the Datadog UI will automatically have the \"service\" key prepended (e.g. <code>service:my-app</code>)",
          :default => "None" %>
          <%= argument 'with_downtimes', "If this argument is set to <code>true</code>, then the returned data will include all current downtimes for each monitor.",
          :default => "true" %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/monitor</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-show-all.py" %>
        <%= snippet_code_block "api-monitor-show-all.rb" %>
        <%= snippet_code_block "api-monitor-show-all.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-show-all.py" %>
        <%= snippet_result_code_block "api-monitor-show-all.rb" %>
        <%= snippet_result_code_block "api-monitor-show-all.sh" %>
      </div>
    </div>

    <!-- Mute all monitors -->
    <div class="int-anchor" id="monitor-mute-all"></div><a href="#monitor-mute-all"><h4>Mute all monitors</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        Muting will prevent all monitors from notifying through email and posts
        to the event stream. State changes will only be visible by checking
        the alert page.
        </p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/monitor/mute_all</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-mute-all.py" %>
        <%= snippet_code_block "api-monitor-mute-all.rb" %>
        <%= snippet_code_block "api-monitor-mute-all.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-mute-all.py" %>
        <%= snippet_result_code_block "api-monitor-mute-all.rb" %>
        <%= snippet_result_code_block "api-monitor-mute-all.sh" %>
      </div>
    </div>

    <!-- Unmute all monitors -->
    <div class="int-anchor" id="monitor-unmute-all"></div><a href="#monitor-unmute-all"><h4>Unmute all monitors</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        Disables muting all monitors. Throws an error if mute all was
        not enabled previously.
        </p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/monitor/unmute_all</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-unmute-all.py" %>
        <%= snippet_code_block "api-monitor-unmute-all.rb" %>
        <%= snippet_code_block "api-monitor-unmute-all.sh" %>
        <h5>Example Response</h5>
        <%= no_response %>
      </div>
    </div>

    <!-- Mute a single monitor -->
    <div class="int-anchor" id="monitor-mute"></div><a href="#monitor-mute"><h4>Mute a monitor</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
          <%= argument 'scope', "The scope to apply the mute to, e.g. role:db",
          :default => "None" %>
          <%= argument 'end', "A POSIX timestamp for when the mute should end",
          :default => "None" %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/monitor/:monitor_id/mute</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-mute.py" %>
        <%= snippet_code_block "api-monitor-mute.rb" %>
        <%= snippet_code_block "api-monitor-mute.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-mute.py" %>
        <%= snippet_result_code_block "api-monitor-mute.rb" %>
        <%= snippet_result_code_block "api-monitor-mute.sh" %>
      </div>
    </div>

    <!-- Unmute a single monitor -->
    <div class="int-anchor" id="monitor-unmute"></div><a href="#monitor-unmute"><h4>Unmute a monitor</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
          <%= argument 'scope', "The scope to apply the mute to. For example,
          if your alert is grouped by {host}, you might mute 'host:app1'",
          :default => "None" %>
          <%= argument 'all_scopes', "Clear muting across all scopes",
          :default => "False" %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/monitor/:monitor_id/unmute</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-unmute.py" %>
        <%= snippet_code_block "api-monitor-unmute.rb" %>
        <%= snippet_code_block "api-monitor-unmute.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-unmute.py" %>
        <%= snippet_result_code_block "api-monitor-unmute.rb" %>
        <%= snippet_result_code_block "api-monitor-unmute.sh" %>
      </div>
    </div>

    <!--
    =====================================================================
    Downtimes
    ====================================================================
    -->

    <div class="int-anchor" id="downtimes"></div><a href="#downtimes"><h3>Downtimes</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>Downtiming gives you greater control over monitor notifications by allowing you
        to globally exclude scopes from alerting. Downtime settings, which can be
        scheduled with start and end times, prevent all alerting related to specified Datadog tags.</p>
      </div>
    </div>

    <!-- Schedule monitor downtime -->
    <div class="int-anchor" id="schedule-downtime"></div><a href="#schedule-downtime"><h4>Schedule monitor downtime</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
          <%= argument 'scope', "The scope(s) to which the downtime will apply, e.g.
          'host:app2'. Provide multiple scopes as a comma-separated list, e.g. 'env:dev,env:prod'. The resulting downtime
          applies to sources that matches ALL provided scopes (i.e. env:dev AND env:prod), NOT any of them." %>
          <%= argument 'monitor_id', "A single monitor to which the downtime will apply. If not provided,
          the downtime will apply to all monitors.", :default=>"None" %>
          <%= argument 'start', "POSIX timestamp to start the downtime. If not provided, the downtime starts the moment
          it is created.", :default=>"None" %>
          <%= argument 'end', "POSIX timestamp to end the downtime. If not provided, the downtime will be in effect indefinitely
          (i.e. until you cancel it).",
          :default => "None" %>
          <%= argument 'message', "A message to include with notifications for this downtime. Email notifications can be sent to specific users by using the same '@username' notation as events",
          :default => "None" %>
          <%= argument 'monitor_id', "The id of a specific monitor to apply the downtime to.", :required => false, :default => "None" %>
          <li>
            <strong>recurrence [optional, default=None]</strong>
            <div>An object defining the recurrence of the downtime with a variety of parameters:</div>
            <ul>
              <li>
                <code>type</code> the type of recurrence. Choose from: <code>days</code>, <code>weeks</code>,
              <code>months</code>, <code>years</code>.
              </li>
              <li>
                <code>period</code> how often to repeat as an integer. For example to repeat every 3 days, select
                a type of <code>days</code> and a period of <code>3</code>.
              </li>
              <li>
                <code>week_days</code> (optional) a list of week days to repeat on. Choose from: <code>Mon</code>, <code>Tue</code>, <code>Wed</code>, <code>Thu</code>, <code>Fri</code>, <code>Sat</code> or <code>Sun</code>. Only applicable when <code>type</code> is <code>weeks</code>. <strong>First letter must be capitalized.</strong>
              </li>
              <li>
                <code>until_occurrences</code> (optional) how many times the downtime will be rescheduled.
                <strong><code>until_occurences</code> and <code>until_date</code></strong> are mutually exclusive
              </li>
              <li>
                <code>until_date</code> (optional) the date at which the recurrence should end as a POSIX timestmap.
                <strong><code>until_occurences</code> and <code>until_date</code></strong> are mutually exclusive
              </li>
            </ul>
          </li>
          <%= argument 'timezone', "The timezone for the downtime.", :default => "UTC" %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/downtime</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-schedule-downtime.py" %>
        <%= snippet_code_block "api-monitor-schedule-downtime.rb" %>
        <%= snippet_code_block "api-monitor-schedule-downtime.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-schedule-downtime.py" %>
        <%= snippet_result_code_block "api-monitor-schedule-downtime.rb" %>
        <%= snippet_result_code_block "api-monitor-schedule-downtime.sh" %>
      </div>
    </div>

    <!-- Update monitor downtime -->
    <div class="int-anchor" id="update-downtime"></div><a href="#update-downtime"><h4>Update monitor downtime</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
          <%= argument 'id', "The integer id of the downtime to be updated" %>
          <%= argument 'scope', "The scope to which the downtime will apply, e.g.
          'host:app2'. Provide multiple scopes as a comma-separated list, e.g. 'env:dev,env:prod'. The resulting downtime
          applies to sources that matches ALL provided scopes (i.e. env:dev AND env:prod), NOT any of them.",
          :default => "original scope" %>
          <%= argument 'monitor_id', "A single monitor to which the downtime will apply. If not provided,
          the downtime will apply to all monitors.", :default=>"original monitor_id" %>
          <%= argument 'start', "POSIX timestamp to start the downtime.",
          :default => "original start" %>
          <%= argument 'end', "POSIX timestamp to end the downtime. If not provided, the downtime will be in effect indefinitely
          (i.e. until you cancel it).",
          :default => "original end" %>
          <%= argument 'message', "A message to include with notifications for this downtime. Email notifications can be sent to specific users by using the same '@username' notation as events",
          :default => "original message" %>
          <li>
            <strong>recurrence [optional, default=original recurrence]</strong>
            <div>An object defining the recurrence of the downtime with a variety of parameters:</div>
            <ul>
              <li>
                <code>type</code> the type of recurrence. Choose from: <code>days</code>, <code>weeks</code>,
              <code>months</code>, <code>years</code>.
              </li>
              <li>
                <code>period</code> how often to repeat as an integer. For example to repeat every 3 days, select
                a type of <code>days</code> and a period of <code>3</code>.
              </li>
              <li>
                <code>week_days</code> (optional) a list of week days to repeat on. Choose from: <code>Mon</code>, <code>Tue</code>, <code>Wed</code>, <code>Thu</code>, <code>Fri</code>, <code>Sat</code> or <code>Sun</code>. Only applicable when <code>type</code> is <code>weeks</code>. <strong>First letter must be capitalized.</strong>
              </li>
              <li>
                <code>until_occurrences</code> (optional) how many times the downtime will be rescheduled.
                <strong><code>until_occurences</code> and <code>until_date</code></strong> are mutually exclusive
              </li>
              <li>
                <code>until_date</code> (optional) the date at which the recurrence should end as a POSIX timestmap.
                <strong><code>until_occurences</code> and <code>until_date</code></strong> are mutually exclusive
              </li>
            </ul>
          </li>
          <%= argument 'timezone', "The timezone for the downtime.", :default => "original timezone" %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT /api/v1/downtime/:downtime_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-update-downtime.py" %>
        <%= snippet_code_block "api-monitor-update-downtime.rb" %>
        <%= snippet_code_block "api-monitor-update-downtime.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-update-downtime.py" %>
        <%= snippet_result_code_block "api-monitor-update-downtime.rb" %>
        <%= snippet_result_code_block "api-monitor-update-downtime.sh" %>
      </div>
    </div>

    <!-- Cancel monitor downtime -->
    <div class="int-anchor" id="cancel-downtime"></div><a href="#cancel-downtime"><h4>Cancel monitor downtime</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <%= argument 'id', "The integer id of the downtime to be canceled" %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/downtime/:downtime_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-cancel-downtime.py" %>
        <%= snippet_code_block "api-monitor-cancel-downtime.rb" %>
        <%= snippet_code_block "api-monitor-cancel-downtime.sh" %>
        <h5>Example Response</h5>
        <%= no_response %>
      </div>
    </div>

    <!-- Cancel monitor downtime by scope -->
    <div class="int-anchor" id="cancel-downtime-by-scope"></div><a href="#cancel-downtime-by-scope"><h4>Cancel monitor downtimes by scope</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <%= argument 'scope', "Cancel all downtimes with the given scope(s), e.g. 'env:prod', 'role:db,role:db-slave'." %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/downtime/cancel/by_scope</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-cancel-downtime-by-scope.sh" %>
        <h5>Example Response</h5>
        <code>{"cancelled_ids":[123456789,123456790]}</code>
      </div>
    </div>

    <!-- Get a monitor downtime -->
    <div class="int-anchor" id="get-downtime"></div><a href="#get-downtime"><h4>Get a monitor downtime</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
          <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/downtime/:downtime_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-get-downtime.py" %>
        <%= snippet_code_block "api-monitor-get-downtime.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-get-downtime.py" %>
        <%= snippet_result_code_block "api-monitor-get-downtime.sh" %>
      </div>
    </div>

    <!-- Get all monitor downtimes -->
    <div class="int-anchor" id="get-all-downtime"></div><a href="#get-all-downtime"><h4>Get all monitor downtimes</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
          <%= argument 'current_only', "Only return downtimes that are active
          when the request is made.", :default => false %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/downtime</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-monitor-get-downtimes.py" %>
        <%= snippet_code_block "api-monitor-get-downtimes.rb" %>
        <%= snippet_code_block "api-monitor-get-downtimes.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-monitor-get-downtimes.py" %>
        <%= snippet_result_code_block "api-monitor-get-downtimes.rb" %>
        <%= snippet_result_code_block "api-monitor-get-downtimes.sh" %>
      </div>
    </div>

    <!--
   =====================================================================
    Timeboards
    ====================================================================
    -->

    <div class="int-anchor" id="timeboards"></div><a href="#timeboards"><h3>Timeboards</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This endpoint allows you to programmatically create, update
          delete and query timeboards.
        </p>
      </div>
    </div>
    <div class="int-anchor" id="dashboards-post"></div><a href="#dashboards-post"><h4>Create a Timeboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("title", 'The name of the dashboard.') %>
          <%= argument("description", "A description of the dashboard\'s content.") %>
          <%= argument("graphs", "A list of graph definitions. Graph definitions follow this form:") %>
          <ul class="arguments">
            <li>
              <strong>title [required]</strong>
              <div>The name of the graph.</div>
            </li>
            <li>
              <strong>definition [required]</strong>
              <div>
                The graph definition. Example:
                <div>
                  <code>
                    {"requests": [{"q": "system.cpu.idle{*} by {host}"}
                  </code>
                </div>
              </div>
            </li>
          </ul>
          <%= argument("template_variables", "A list of template variables for using Dashboard templating. Template variable definitions follow this form:", :default => "None") %>
           <ul class="arguments">
             <li>
               <strong>name [required]</strong>
               <div>The name of the variable.</div>
             </li>
             <li>
               <strong>prefix [optional, default=None]</strong>
               <div>
                 The tag prefix associated with the variable.  Only tags with this prefix will appear in the variable dropdown.
               </div>
             </li>
             <li>
               <strong>default [optional, default=None]</strong>
               <div>The default value for the template variable on dashboard load</div>
             </li>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/dash</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-dashboard-create.py" %>
        <%= snippet_code_block "api-dashboard-create.rb" %>
        <%= snippet_code_block "api-dashboard-create.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-dashboard-create.py" %>
        <%= snippet_result_code_block "api-dashboard-create.rb" %>
        <%= snippet_result_code_block "api-dashboard-create.sh" %>
      </div>
    </div>

    <!-- update a dashboard -->
    <div class="int-anchor" id="dashboards-update"></div><a href="#dashboards-update"><h4>Update a Timeboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <li>
            <strong>title [required]</strong>
            <div>The name of the dashboard.</div>
          </li>
          <li>
            <strong>description [required]</strong>
            <div>A description of the dashboard's contents.</div>
          </li>
          <li>
            <strong>graphs [required]</strong>
            <div>A list of graph definitions. Graph definitions follow this form:</div>
          </li>
          <ul class="arguments">
            <li>
              <strong>title [required]</strong>
              <div>The name of the graph.</div>
            </li>
            <li>
              <strong>definition [required]</strong>
              <div>
                The graph definition. Read the <a href="/graphing/">Graph
                Guide</a> for more on graphs. Example:
                <div>
                  <code>
                    {"requests": [{"q": "system.cpu.idle{*} by {host}"}
                  </code>
                </div>
              </div>
            </li>
          </ul>
        <%= argument("template_variables", "A list of template variables for using Dashboard templating. Template variable definitions follow this form:", :default => "None") %>
           <ul class="arguments">
             <li>
               <strong>name [required]</strong>
               <div>The name of the variable.</div>
             </li>
             <li>
               <strong>prefix [optional, default=None]</strong>
               <div>
                 The tag prefix associated with the variable.  Only tags with this prefix will appear in the variable dropdown.
               </div>
             </li>
             <li>
               <strong>default [optional, default=None]</strong>
               <div>The default value for the template variable on dashboard load</div>
             </li>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT /api/v1/dash/:dash_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-dashboard-update.py" %>
        <%= snippet_code_block "api-dashboard-update.rb" %>
        <%= snippet_code_block "api-dashboard-update.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-dashboard-update.py" %>
        <%= snippet_result_code_block "api-dashboard-update.rb" %>
        <%= snippet_result_code_block "api-dashboard-update.sh" %>
      </div>
    </div>

    <!-- delete a dashboard -->
    <div class="int-anchor" id="dashboards-delete"></div><a href="#dashboards-delete"><h4>Delete a Timeboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Delete an existing timeboard.</p>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/dash/:dash_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-dashboard-delete.py" %>
        <%= snippet_code_block "api-dashboard-delete.rb" %>
        <%= snippet_code_block "api-dashboard-delete.sh" %>
        <h5>Example Response</h5>
        <%= no_response %>
      </div>
    </div>

    <!-- get a dashboard -->
    <div class="int-anchor" id="dashboards-get-all"></div><a href="#dashboards-get-all"><h4>Get all Timeboards</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Fetch all of your timeboards' definitions.</p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/dash</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-dashboard-get-all.sh" %>
        <%= snippet_code_block "api-dashboard-get-all.rb" %>
        <%= snippet_code_block "api-dashboard-get-all.py" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-dashboard-get-all.sh" %>
        <%= snippet_result_code_block "api-dashboard-get-all.rb" %>
        <%= snippet_result_code_block "api-dashboard-get-all.py" %>
      </div>
    </div>

    <!-- get a dashboard -->
    <div class="int-anchor" id="dashboards-get"></div><a href="#dashboards-get"><h4>Get a Timeboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Fetch an existing dashboard's definition.</p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/dash/:dash_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-dashboard-get.py" %>
        <%= snippet_code_block "api-dashboard-get.rb" %>
        <%= snippet_code_block "api-dashboard-get.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-dashboard-get.py" %>
        <%= snippet_result_code_block "api-dashboard-get.rb" %>
        <%= snippet_result_code_block "api-dashboard-get.sh" %>
      </div>
    </div>

   <!--
    =====================================================================
    Screenboards
    ====================================================================
    -->

    <div class="int-anchor" id="screenboards"></div><a href="#screenboards"><h3>Screenboards</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        You can view more detailed documentation on the Screenboard API at
        <a href="/api/screenboards/">http://docs.datadoghq.com/api/screenboards/</a>.
        </p>
      </div>
    </div>

    <div class="int-anchor" id="screenboards-post"></div><a href="#screenboards-post"><h4>Create a Screenboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("board_title", 'The name of the dashboard.') %>
          <%= argument("description", "A description of the dashboard\'s content.", :default => "None") %>
          <%= argument("widgets", "A list of widget definitions. See <a href='/api/screenboards/'>here</a> for more examples.") %>
          <%= argument("template_variables", "A list of template variables for using Dashboard templating.", :default => "None") %>
          <%= argument("width", "Screenboard width in pixels", :default => "None") %>
          <%= argument("height", "Height in pixels.", :default => "None") %>
          <%= argument("read_only", "The read-only status of the screenboard.", :default => "False") %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/screen</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-create.py" %>
        <%= snippet_code_block "api-screenboard-create.rb" %>
        <%= snippet_code_block "api-screenboard-create.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-screenboard-create.py" %>
        <%= snippet_result_code_block "api-screenboard-create.rb" %>
        <%= snippet_result_code_block "api-screenboard-create.sh" %>
      </div>
    </div>

    <div class="int-anchor" id="screenboards-put"></div><a href="#screenboards-put"><h4>Update a Screenboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("board_title", 'The name of the dashboard.') %>
          <%= argument("description", "A description of the dashboard\'s content.", :default => "None") %>
          <%= argument("widgets", "A list of widget definitions. See <a href='/api/screenboards/'>here</a> for more examples.") %>
          <%= argument("template_variables", "A list of template variables for using Dashboard templating.", :default => "None") %>
          <%= argument("width", "Screenboard width in pixels", :default => "None") %>
          <%= argument("height", "Height in pixels.", :default => "None") %>
          <%= argument("read_only", "The read-only status of the screenboard.", :default => "False") %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT /api/v1/screen/:board_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-update.py" %>
        <%= snippet_code_block "api-screenboard-update.rb" %>
        <%= snippet_code_block "api-screenboard-update.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-screenboard-update.py" %>
        <%= snippet_result_code_block "api-screenboard-update.rb" %>
        <%= snippet_result_code_block "api-screenboard-update.sh" %>
      </div>
    </div>


    <!-- delete a dashboard -->
    <div class="int-anchor" id="timeboards-delete"></div><a href="#timeboards-delete"><h4>Delete a Screenboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Delete an existing screenboard.</p>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/screen/:board_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-delete.py" %>
        <%= snippet_code_block "api-screenboard-delete.rb" %>
        <%= snippet_code_block "api-screenboard-delete.sh" %>
        <h5>Example Response</h5>
        <!-- TO FIX -->
        <%= no_response %>
      </div>
    </div>

    <!-- get a screenboard -->
    <div class="int-anchor" id="screenboards-get"></div><a href="#screenboards-get"><h4>Get a Screenboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Fetch an existing screenboard's definition.</p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/screen/:board_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-get.py" %>
        <%= snippet_code_block "api-screenboard-get.rb" %>
        <%= snippet_code_block "api-screenboard-get.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-screenboard-get.py" %>
        <%= snippet_result_code_block "api-screenboard-get.rb" %>
        <%= snippet_result_code_block "api-screenboard-get.sh" %>
      </div>
    </div>

    <!-- get all screenboards -->
    <div class="int-anchor" id="screenboards-get-all"></div><a href="#screenboards-get-all"><h4>Get all Screenboards</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Fetch all of your screenboards' definitions.</p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/screen</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-get-all.py" %>
        <%= snippet_code_block "api-screenboard-get-all.rb" %>
        <%= snippet_code_block "api-screenboard-get-all.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-screenboard-get-all.py" %>
        <%= snippet_result_code_block "api-screenboard-get-all.rb" %>
        <%= snippet_result_code_block "api-screenboard-get-all.sh" %>
      </div>
    </div>


    <!-- share a screenboard -->
    <div class="int-anchor" id="screenboards-share"></div><a href="#screenboards-share"><h4>Share a Screenboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Share an existing screenboard's with a public URL.</p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/screen/share/:board_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-share.py" %>
        <%= snippet_code_block "api-screenboard-share.rb" %>
        <%= snippet_code_block "api-screenboard-share.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-screenboard-share.py" %>
        <%= snippet_result_code_block "api-screenboard-share.rb" %>
        <%= snippet_result_code_block "api-screenboard-share.sh" %>
      </div>
    </div>

    <!-- revoke a screenboard -->
    <div class="int-anchor" id="screenboards-revoke"></div><a href="#screenboards-revoke"><h4>Revoke a shared a Screenboard</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>Revoke a currently shared screenboard's.</p>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/screen/share/:board_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-screenboard-revoke.py" %>
        <%= snippet_code_block "api-screenboard-revoke.rb" %>
        <%= snippet_code_block "api-screenboard-revoke.sh" %>
        <h5>Example Response</h5>
        <%= no_response %>
      </div>
    </div>


    <!--
   =====================================================================
    Hosts
    ====================================================================
    -->
    <div class="int-anchor" id="hosts"></div><a href="#hosts"><h3>Hosts</h3></a>

    <div class="int-anchor" id="hosts-mute"></div><a href="#hosts-mute"><h4>Mute a Host</h4></a>
    <div class="row">
    <%= left_side_div %>
      <h5>Arguments</h5>
      <ul class="arguments">
        <%= argument("end", "POSIX timestamp when the host will be unmuted. If omitted, the host will remain muted until explicitly unmuted.", {:default => 'None'}) %>
        <%= argument("message", "Message to associate with the muting of this host", {:default => 'None'}) %>
        <%= argument("override", "If true and the host is already muted, will replace existing host mute settings.", {:default => 'False'}) %>
      </ul>
    </div>
    <%= right_side_div %>
    <h5>Signature</h5>
      <code>POST /api/v1/host/:hostname/mute</code>
      <h5>Example Request</h5>
      <%= snippet_code_block "api-host-mute.rb" %>
      <%= snippet_code_block "api-host-mute.py" %>
      <%= snippet_code_block "api-host-mute.sh" %>
      <h5>Example Response</h5>
      <%= snippet_result_code_block "api-host-mute.rb" %>
      <%= snippet_result_code_block "api-host-mute.py" %>
      <%= snippet_result_code_block "api-host-mute.sh" %>
      </div>
    </div>

    <div class="int-anchor" id="hosts-unmute"></div><a href="#hosts-unmute"><h4>Unmute a Host</h4></a>
    <div class="row">
    <%= left_side_div %>
      <h5>Arguments</h5>
        <%= no_args %>
    </div>
    <%= right_side_div %>
    <h5>Signature</h5>
      <code>POST /api/v1/host/:hostname/unmute</code>
      <h5>Example Request</h5>
      <%= snippet_code_block "api-host-unmute.rb" %>
      <%= snippet_code_block "api-host-unmute.py" %>
      <%= snippet_code_block "api-host-unmute.sh" %>
      <h5>Example Response</h5>
      <%= snippet_result_code_block "api-host-unmute.rb" %>
      <%= snippet_result_code_block "api-host-unmute.py" %>
      <%= snippet_result_code_block "api-host-unmute.sh" %>
      </div>
    </div>

    <!--
   =====================================================================
    Tags
    ====================================================================
    -->

    <div class="int-anchor" id="tags"></div><a href="#tags"><h3>Tags</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          The tag end point allows you to tag hosts with keywords meaningful to you - like <code>role:database</code>.
          All metrics sent from a host will have its tags applied.

          When fetching and applying tags to a particular host, you can refer
          to hosts by name (<code>yourhost.example.com</code>).
        </p>
        <p>
        The component of your infrastructure responsible for a tag is identified by
        a <code>source</code>. Valid sources are: nagios, hudson, jenkins, users,
        feed, chef, puppet, git, bitbucket, fabric, capistrano.
        </p>
      </div>
    </div>

    <div class="int-anchor" id="tags-get"></div><a href="#tags-get"><h4>Get Tags</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Return a mapping of tags to hosts for your whole infrastructure.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("source", "Only show tags from a particular source. Otherwise shows all tags.", :default => "None") %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/tags/hosts</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-tags-get.rb" %>
        <%= snippet_code_block "api-tags-get.py" %>
        <%= snippet_code_block "api-tags-get.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-tags-get.rb" %>
        <%= snippet_result_code_block "api-tags-get.py" %>
        <%= snippet_result_code_block "api-tags-get.sh" %>
      </div>
    </div>

    <div class="int-anchor" id="tags-get-host"></div><a href="#tags-get-host"><h4>Get Host Tags</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Return the list of tags that apply to a given host.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("source", "Only show tags from a particular source. Otherwise shows all tags.", :default => "None") %>
          <%= argument("by_source", "Return tags grouped by source.", :default => "False") %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/tags/hosts/:host_name</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-tags-get-host.py" %>
        <%= snippet_code_block "api-tags-get-host.rb" %>
        <%= snippet_code_block "api-tags-get-host.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-tags-get-host.py" %>
        <%= snippet_result_code_block "api-tags-get-host.rb" %>
        <%= snippet_result_code_block "api-tags-get-host.sh" %>
      </div>
    </div>

    <div class="int-anchor" id="tags-add"></div><a href="#tags-add"><h4>Add Tags to a Host</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to add tags to a host.
        </p>
         <h5>Arguments</h5>
          <ul class="arguments">
          <%= argument("tags", "A list of tags to apply to the host") %>
          <%= argument("source", "The source of the tags (e.g. chef, puppet).", :default => "users") %>
          </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST /api/v1/tags/hosts/:host_name</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-tags-add.py" %>
        <%= snippet_code_block "api-tags-add.sh" %>
        <%= snippet_code_block "api-tags-add.rb" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-tags-add.py" %>
        <%= snippet_result_code_block "api-tags-add.sh" %>
        <%= snippet_result_code_block "api-tags-add.rb" %>
      </div>
    </div>

    <div class="int-anchor" id="tags-update"></div><a href="#tags-update"><h4>Update Host Tags</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to update all tags for a given host.
        </p>
         <h5>Arguments</h5>
          <ul class="arguments">
          <%= argument("tags", "A list of tags") %>
          <%= argument("source", "The source of the tags (e.g. chef, puppet).", :default => "users") %>
          </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT /api/v1/tags/hosts/:host_name</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-tags-update.py" %>
        <%= snippet_code_block "api-tags-update.sh" %>
        <%= snippet_code_block "api-tags-update.rb" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-tags-update.py" %>
        <%= snippet_result_code_block "api-tags-update.sh" %>
        <%= snippet_result_code_block "api-tags-update.rb" %>
      </div>
    </div>

    <div class="int-anchor" id="tags-remove"></div><a href="#tags-remove"><h4>Remove Host Tags</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to remove all tags for a given host.
        </p>
         <h5>Arguments</h5>
          <ul class="arguments">
          <%= argument("source", "The source of the tags (e.g. chef, puppet).", :default => "users") %>
          </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE /api/v1/tags/hosts/:host_name</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-tags-remove.py" %>
        <%= snippet_code_block "api-tags-remove.sh" %>
        <%= snippet_code_block "api-tags-remove.rb" %>
        <h5>Example Response</h5>
        <%= no_response %>
      </div>
    </div>

    <!--
   =====================================================================
    Search
    ====================================================================
    -->

    <div class="int-anchor" id="search"></div><a href="#search"><h3>Search</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          This end point allows you to search for entities from the last 24 hours in Datadog. The
          currently searchable entities are:
        </p>
        <ul>
          <li><code>hosts  </code></li>
          <li><code>metrics</code></li>
        </ul>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument("q", "The query string") %>
        </ul>
        <h5>Query Language</h5>
        <p>
          Search queries allow for limited faceting. Available facets are:
        </p>
        <ul>
          <li><code>hosts  </code></li>
          <li><code>metrics</code></li>
        </ul>
        <p>
          Faceting your search limits your results to only matches of the specified
          type. Un-faceted queries return results for all possible types.
        </p>
        <p>
          Un-faceted queries are of the form:
        </p>
        <code>query_string</code>
        <p>
          Faceted queries are of the form:
        </p>
        <code>facet:query_string</code>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET /api/v1/search</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-search.py" %>
        <%= snippet_code_block "api-search.rb" %>
        <%= snippet_code_block "api-search.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-search.py" %>
        <%= snippet_result_code_block "api-search.rb" %>
        <%= snippet_result_code_block "api-search.sh" %>
      </div>
    </div>

    <!--
   =====================================================================
    Comments
    ====================================================================
    -->

    <div class="int-anchor" id="comments"></div><a href="#comments"><h3>Comments</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        Comments are how discussion happens on Datadog. You can create, edit, delete
        and reply to comments.
        </p>
      </div>
    </div>

    <!-- Create comments -->
    <div class="int-anchor" id="comments-create"></div><a href="#comments-create"><h4>Create a comment</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Comments are essentially special forms of events that
          appear in the stream. They can start a new discussion thread or
          optionally, reply in another thread.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'message', "The comment text." %>
          <%= argument 'handle', "The handle of the user making the comment.", :default => 'application key owner' %>
          <%= argument 'related_event_id', "The id of another comment or event to
          reply to", :default => 'None' %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST api/v1/comments</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-comment-create.py" %>
        <%= snippet_code_block "api-comment-create.rb" %>
        <%= snippet_code_block "api-comment-create.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-comment-create.py" %>
        <%= snippet_result_code_block "api-comment-create.rb" %>
        <%= snippet_result_code_block "api-comment-create.sh" %>
      </div>
    </div>

    <!-- Edit comments -->
    <div class="int-anchor" id="comments-edit"></div><a href="#comments-edit"><h4>Edit a Comment</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'message', "The comment text.", :default => 'original message' %>
          <%= argument 'handle', "The handle of the user making the comment.", :default => 'application key owner' %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT api/v1/comments/:comment_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-comment-edit.py" %>
        <%= snippet_code_block "api-comment-edit.rb" %>
        <%= snippet_code_block "api-comment-edit.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-comment-edit.py" %>
        <%= snippet_result_code_block "api-comment-edit.rb" %>
        <%= snippet_result_code_block "api-comment-edit.sh" %>
      </div>
    </div>

    <!-- Edit comments -->
    <div class="int-anchor" id="comments-delete"></div><a href="#comments-delete"><h4>Delete a Comment</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <%= no_args %>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE api/v1/comments/:comment_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-comment-delete.py" %>
        <%= snippet_code_block "api-comment-delete.rb" %>
        <%= snippet_code_block "api-comment-delete.sh" %>
        <h5>Example Response</h5>
        <%= no_response %>
      </div>
    </div>

    <!--
    =====================================================================
    Users
    ====================================================================
    -->

    <div class="int-anchor" id="users"></div><a href="#users"><h3>Users</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        You can create, edit, and disable users.
        </p>
      </div>
    </div>

    <!-- Create user -->
    <div class="int-anchor" id="user-create"></div><a href="#user-create"><h4>Create User</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'handle', "The user handle, must be a valid email." %>
          <%= argument 'name', "The name of the user.", :default => 'None' %>
          <%= argument 'access_role', "The access role of the user. Choose from <code>'st'</code> (standard user), <code>'adm'</code> (admin user), or <code>'ro'</code> (read-only user). <em>Note: users can be created with admin access role only with application keys belonging to administrators.</em>", :default => 'st' %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST api/v1/user</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-user-create.py" %>
        <%= snippet_code_block "api-user-create.rb" %>
        <%= snippet_code_block "api-user-create.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-user-create.py" %>
        <%= snippet_result_code_block "api-user-create.rb" %>
        <%= snippet_result_code_block "api-user-create.sh" %>
      </div>
    </div>

    <!-- Get User -->
    <div class="int-anchor" id="user-get"></div><a href="#user-get"><h4>Get User</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'handle', "The handle of the user." %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/user/:handle</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-user-get.py" %>
        <%= snippet_code_block "api-user-get.rb" %>
        <%= snippet_code_block "api-user-get.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-user-get.py" %>
        <%= snippet_result_code_block "api-user-get.rb" %>
        <%= snippet_result_code_block "api-user-get.sh" %>
      </div>
    </div>

    <!-- Get All Users -->
    <div class="int-anchor" id="user-get-all"></div><a href="#user-get-all"><h4>Get All Users</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= no_args %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/user </code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-user-get-all.py" %>
        <%= snippet_code_block "api-user-get-all.rb" %>
        <%= snippet_code_block "api-user-get-all.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-user-get-all.py" %>
        <%= snippet_result_code_block "api-user-get-all.rb" %>
        <%= snippet_result_code_block "api-user-get-all.sh" %>
      </div>
    </div>


    <!-- Update user -->
    <div class="int-anchor" id="user-update"></div><a href="#user-update"><h4>Update User</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Can only be used with application keys belonging to administrators.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'handle', "The handle of the user." %>
          <%= argument 'name', "The new name of the user.", :default => 'None' %>
          <%= argument 'email', "The new email of the user.", :default => 'None' %>
          <%= argument 'disabled', "The new disabled status of the user.", :default => 'None' %>
          <%= argument 'access_role', "The new access role of the user. Choose from <code>'st'</code> (standard user), <code>'adm'</code> (admin user), or <code>'ro'</code> (read-only user).", :default => 'st' %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>PUT api/v1/user/:handle</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-user-update.py" %>
        <%= snippet_code_block "api-user-update.rb" %>
        <%= snippet_code_block "api-user-update.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-user-update.py" %>
        <%= snippet_result_code_block "api-user-update.rb" %>
        <%= snippet_result_code_block "api-user-update.sh" %>
      </div>
    </div>


    <!-- Disable User -->
    <div class="int-anchor" id="user-disable"></div><a href="#user-disable"><h4>Disable User</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Can only be used with application keys belonging to administrators.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument 'handle', "The handle of the user." %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>DELETE api/v1/user/:handle</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-user-disable.py" %>
        <%= snippet_code_block "api-user-disable.rb" %>
        <%= snippet_code_block "api-user-disable.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-user-disable.py" %>
        <%= snippet_result_code_block "api-user-disable.rb" %>
        <%= snippet_result_code_block "api-user-disable.sh" %>
      </div>
    </div>

    <!--
    =====================================================================
    Graph Snapshot
    ====================================================================
    -->

    <div class="int-anchor" id="graphs"></div><a href="#graphs"><h3>Graphs</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
        You can take graph snapshots using the API.
        </p>
      </div>
    </div>

    <!-- Create alerts -->
    <div class="int-anchor" id="graph-snapshot"></div><a href="#graph-snapshot"><h4>Graph Snapshot</h4></a>
    <div class="row">
      <%= left_side_div %>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument "metric_query", "The metric query." %>
          <%= argument "start", "The POSIX timestamp of the start of the query." %>
          <%= argument "end", "The POSIX timestamp of the end of the query." %>
          <%= argument "event_query", "A query that will add event bands to the graph.", {:default => 'None'} %>
          <%= argument "graph_def", "A JSON document defining the graph. <code>graph_def</code> can be used instead of <code>metric_query</code>. The JSON document uses the <a href='/graphingjson/#grammar'>grammar defined here</a> and should be formatted to a single line then URLEncoded. The <code>graph_def</code> argument is only available in the REST API and not using the Ruby or Python wrappers. ",  {:default => 'None'} %>
          <%= argument "title", "A title for the graph. If no title is specified, the graph will not have a title.", {:default => 'None'} %>
        </ul>

      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/graph/snapshot</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-graph-snapshot.py" %>
        <%= snippet_code_block "api-graph-snapshot.rb" %>
        <%= snippet_code_block "api-graph-snapshot.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-graph-snapshot.py" %>
        <%= snippet_result_code_block "api-graph-snapshot.rb" %>
        <%= snippet_result_code_block "api-graph-snapshot.sh" %>
      </div>
    </div>

    <!--
    =====================================================================
    Embeddable Graphs
    ====================================================================
    -->

    <div class="int-anchor" id="embeds"></div><a href="#embeds"><h3>Embeddable Graphs</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          You can interact with embeddable graphs through the API.
        </p>
      </div>
    </div>

    <!-- Get All Embeds -->
    <div class="int-anchor" id="embeds-get-all"></div><a href="#embeds-get-all"><h4>Get All Embeds</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Gets a list of previously created embeddable graphs.
        </p>
        <p>
          Returns: A JSON list containing information on previously created embeds from both the UI
          and the API. Each JSON graph response is in the same format as returned by
          <code>GET api/v1/graph/embed/:embed_id</code>.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= no_args %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/graph/embed</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-embeds-get-all.py" %>
        <%= snippet_code_block "api-embeds-get-all.rb" %>
        <%= snippet_code_block "api-embeds-get-all.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-embeds-get-all.py" %>
        <%= snippet_result_code_block "api-embeds-get-all.rb" %>
        <%= snippet_result_code_block "api-embeds-get-all.sh" %>
      </div>
    </div>

    <!-- Create Embed -->
    <div class="int-anchor" id="embeds-create"></div><a href="#embeds-create"><h4>Create Embed</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Creates a new embeddable graph.
        </p>
        <p>
          Returns: A JSON consisting of the same elements returned by <code>GET api/v1/graph/embed/:embed_id</code>.
          On failure, the return value will be a JSON containing an error message <code>{errors: [messages]}</code>.
        </p>
        <p>
          Note: If an embed already exists for the exact same query in a given organization, the older embed will be returned instead of creating a new embed.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument "graph_json", "The graph definition in JSON.
          Same format that is available on the JSON tab of the graph editor" %>
          <%= argument "timeframe", "The timegrame for the graph. Must be one of 1_hour,
          4_hours, 1_day, 2_days, and 1_week.", {:default => '1_hour'} %>
          <%= argument "size", "The size of the graph. Must be one of small, medium,
          large, and xlarge.", {:default => 'medium'} %>
          <%= argument "legend", "The flag determining if the graph includes a legend.
          Must be one of yes or no.", {:default => 'no'} %>
          <%= argument "title", "Determines graph title. Must be at least 1 character.",
          {:default => 'Embed created through API'} %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>POST api/v1/graph/embed</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-embeds-create.py" %>
        <%= snippet_code_block "api-embeds-create.rb" %>
        <%= snippet_code_block "api-embeds-create.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-embeds-create.py" %>
        <%= snippet_result_code_block "api-embeds-create.rb" %>
        <%= snippet_result_code_block "api-embeds-create.sh" %>
      </div>
    </div>

    <!-- Get Specifc Embed -->
    <div class="int-anchor" id="embeds-get"></div><a href="#embeds-get"><h4>Get Specific Embed</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Get the HTML fragment for a previously generated embed with embed_id.
        </p>
        <p>
          Returns: A JSON object with 8 elements:
        </p>
        <ul>
          <li>embed_id: Token of the embed</li>
          <li>graph_title: Tile of the graph</li>
          <li>dash_name: Name of the dashboard the graph is on (null if none)</li>
          <li>dash_url: URL of the dashboard the graph is on (null if none)</li>
          <li>shared_by: ID of the use who shared the embed</li>
          <li>html: HTML fragment for the embed (iframe)</li>
          <li>revoked: Boolean flag for whther or not the embed is revoked</li>
        </ul>
        <p>
          On failure, the return value will be a JSON containing an error message <code>{errors: [messages]}</code>.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= argument "size", "The size of the graph. Must be one of small, medium,
          large, and xlarge.", {:default => 'medium'} %>
          <%= argument "legend", "The flag determining if the graph includes a legend.
          Must be one of yes or no.", {:default => 'no'} %>
          <%= argument "template_variables", "Replace template variables in queries with form $var. To
          replace $var with val, use var=val as a parameter for each template variable you wish
          to replace. If any template variables are missing values in the iframe source
          url, then (*) will be used as the value.", {:default => "None"} %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/graph/embed/:embed_id</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-embeds-get.py" %>
        <%= snippet_code_block "api-embeds-get.rb" %>
        <%= snippet_code_block "api-embeds-get.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-embeds-get.py" %>
        <%= snippet_result_code_block "api-embeds-get.rb" %>
        <%= snippet_result_code_block "api-embeds-get.sh" %>
      </div>
    </div>

    <!-- Enable Embed -->
    <div class="int-anchor" id="embeds-enable"></div><a href="#embeds-enable"><h4>Enable Embed</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Enable a specified embed.
        </p>
        <p>
          Returns: A JSON containing the success message <code>{success: [message]}</code>.
          On failure, the return value will be a JSON containing an error message <code>{errors: [messages]}</code>.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= no_args %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/graph/embed/:embed_id/enable</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-embeds-enable.py" %>
        <%= snippet_code_block "api-embeds-enable.rb" %>
        <%= snippet_code_block "api-embeds-enable.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-embeds-enable.py" %>
        <%= snippet_result_code_block "api-embeds-enable.rb" %>
        <%= snippet_result_code_block "api-embeds-enable.sh" %>
      </div>
    </div>

    <!-- Revoke Embed -->
    <div class="int-anchor" id="embeds-revoke"></div><a href="#embeds-revoke"><h4>Revoke Embed</h4></a>
    <div class="row">
      <%= left_side_div %>
        <p>
          Revoke a specified embed.
        </p>
        <p>
          Returns: A JSON containing the success message <code>{success: [message]}</code>.
          On failure, the return value will be a JSON containing an error message <code>{errors: [messages]}</code>.
        </p>
        <h5>Arguments</h5>
        <ul class="arguments">
          <%= no_args %>
        </ul>
      </div>
      <%= right_side_div %>
        <h5>Signature</h5>
        <code>GET api/v1/graph/embed/:embed_id/revoke</code>
        <h5>Example Request</h5>
        <%= snippet_code_block "api-embeds-revoke.py" %>
        <%= snippet_code_block "api-embeds-revoke.rb" %>
        <%= snippet_code_block "api-embeds-revoke.sh" %>
        <h5>Example Response</h5>
        <%= snippet_result_code_block "api-embeds-revoke.py" %>
        <%= snippet_result_code_block "api-embeds-revoke.rb" %>
        <%= snippet_result_code_block "api-embeds-revoke.sh" %>
      </div>
    </div>

   <!--
    =====================================================================
    Troubleshooting
    ====================================================================
    -->

    <div class="int-anchor" id="troubleshooting"></div><a href="#troubleshooting"><h3>Troubleshooting</h3></a>
    <div class="row">
      <%= left_side_div %>
        <p>We do very minimal error checking on the API front-end, as we queue all data for asynchronous processing
           (the goal being to always, always accept your data in production situations and decouple our systems from yours).</p>
        <p>Thus it is possible you could receive a 202 'success' response but not see your data in Datadog.
              The cause of this is most likely:</p>
        <ul>
          <li>Problems with the timestamp (either not in seconds or in the past, etc.)</li>
          <li>Using the application key instead of API key</li>
          <li>Events are succeeding, but because success events are low priority, they don't show up in the event stream until it is switched to priority 'all'</li>
        </ul>
        <p>To check your timestamp is correct run:<br> <code>date -u && curl -s -v https://app.datadoghq.com 2>&1 | grep Date</code>
        <br>
        This will output the current system’s date, and then make a request to our endpoint and grab
               the date on our end. If these are more than a few minutes apart,
               you may want to look at the time settings on your server.</p>
        <br>
        <p>
         There are also certain fields which are not mandatory for submission, but do require a valid input.
         For example, in submitting an event the <code>priority</code> field must be one of the four given options.
         Any other text will result in a 202 'success' but no event showing up.
         Having an invalid <code>source_type_name</code> will not prevent the event from showing up, but that field will be dropped upon submission.
        </p>


  </div>

</div>

<% content_for :javascript do %>
  <script type="text/javascript">
    $(DD_docs.apiPage);
  </script>
<% end %>